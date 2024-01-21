# Custom Transactional Decorator Challenger

## 문제

* NestJS에서 TypeORM을 사용하여 트랜잭션을 구현할 때, 하나의 메서드가 너무 많은 책임을 가지고 있다.
* 중복된 코드가 존재한다.

{% hint style="info" %}
NestJS 공식 문서에서는 Transactions를 구현하는 두가지 방법

* QueryRunner 사용

```typescript
async createMany(users: User[]) {
  const queryRunner = this.dataSource.createQueryRunner();

  await queryRunner.connect();
  await queryRunner.startTransaction();
  try {
    await queryRunner.manager.save(users[0]);
    await queryRunner.manager.save(users[1]);

    await queryRunner.commitTransaction();
  } catch (err) {
    // since we have errors lets rollback the changes we made
    await queryRunner.rollbackTransaction();
  } finally {
    // you need to release a queryRunner which was manually instantiated
    await queryRunner.release();
  }
}
```

* dataSource.transaction

```typescript
async createMany(users: User[]) {
  await this.dataSource.transaction(async manager => {
    await manager.save(users[0]);
    await manager.save(users[1]);
  });
}
```
{% endhint %}

### 코드

```javascript
async updateInfo(
    data: EditUserInfoData,
  ): Promise<UserInfoResponse> {
    // QueryRunner Connect
    const queryRunner = this.dataSource.createQueryRunner();
    await queryRunner.connect();  
    
    // Transaction Start
    await queryRunner.startTransaction();
    
    try {
    
    // 실제 로직
      const userAccount = await queryRunner.manager.findOneBy(UserAccount, {
        id: id,
        deleted: false,
      });
      if (!userAccount) {
        throw new UserAccountNotFoundError();
      }
    
      userAccount.nickname = data.nickname;
      userAccount.address = data.address;
      userAccount.introduction = data.introduction
        ? data.introduction
        : userAccount.introduction;
      userAccount.updatedAt = new Date();
      await queryRunner.manager.save(UserAccount, userAccount);
      return {
        nickname: userAccount.nickname.value,
        address: userAccount.address.value,
        introduction: userAccount.introduction?.value,
      };
      
      
    } catch (err) {
    // Rollback
      await queryRunner.rollbackTransaction();
    } finally {
    // Release
      await queryRunner.release()
    }
  }
```

위 코드에서 `try {}` 문 외의 코드 중복된다.

## 해결 방법

* TypeScript의 데코레이터를 사용하여 공통 부분을 분리하는 것이 가능할 것으로 보인다.

## 해결 과정

### Step1

#### `@Transactional()` Decorator를 구현해서 공통 로직을 Decorator에서 구현하도록 했다.

* 비지니스 로직

```typescript
@Transactional()
async updateInfo(data: EditUserInfoData) {
  const userAccount = await this.findById(data.userId);
  userAccount.nickname = data.nickname;
  userAccount.address = data.address;
  const now = new Date();
  userAccount.updatedAt = now;
  return this.userAccountRepository.save(userAccount, { transaction: false });
}
```

* 공통 로직(Transactional Decorator)

```typescript
export function Transactional(
  isolationLevel?: IsolationLevel,
): MethodDecorator {
  return (
    _target: any,
    _propertyKey: string,
    descriptor: PropertyDescriptor,
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = async function (...args: any[]) {
      const dataSource: DataSource = this.dataSource;
      if (!dataSource) {
        throw new Error('DataSource is not injected');
      }

      // 트랜잭션을 이미 사용하는 경우 추가로 트랜잭션을 생성하지 않는다.
      const existsTransaction = args.find(
        (arg) => arg.connection !== undefined,
      );
      if (existsTransaction) {
        return originalMethod.apply(this, args);
      }

      // 트랜잭션을 사용하지 않는 경우 트랜잭션을 생성한다.
      const queryRunner: QueryRunner = dataSource.createQueryRunner();
      await queryRunner.connect();
      await queryRunner.startTransaction(
        isolationLevel ? isolationLevel : DEFAULT_ISOLATION_LEVEL,
      );

      try {
        args = args.concat(queryRunner);
        const result = await originalMethod.apply(this, args);
        await queryRunner.commitTransaction();
        return result;
      } catch (err) {
        await queryRunner.rollbackTransaction();
        throw err;
      } finally {
        await queryRunner.release();
      }
    };
  };
}
```

### Step1 문제점

#### 비즈니스 로직의 Connect와 공통 로직의 Connect가 서로 다르기 때문에, 비즈니스 로직을 트랜잭션으로 감싸는 것이 목표였으나 이를 달성하지 못했다.

* 공통 로직은 Connect 47
* 비지니스 로직은 Conenct 48

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### Step2

비즈니스 로직에서 QueryRunner를 사용하여 구현하고, 데코레이터를 통해 이미 트랜잭션이 사용 중인지 확인하고, 필요에 따라 추가적으로 트랜잭션을 시작하거나 QueryRunner를 주입한다.

* 비지니스 로직

```typescript
@Transactional()
async updateInfo(data: EditUserInfoData, queryRunner?: QueryRunner) {
  const userAccount = await this.findByIdWithQueryRunner(
    data.userId,
    queryRunner,
  );
  userAccount.nickname = data.nickname;
  userAccount.address = data.address;
  userAccount.introduction = data.introduction
    ? data.introduction
    : userAccount.introduction;
  const now = new Date();
  userAccount.updatedAt = now;
  await this.saveWithQueryRunner(userAccount, queryRunner);
  return {
    nickname: userAccount.nickname.value,
    address: userAccount.address.value,
    introduction: userAccount.introduction?.value,
  };
}


async findByIdWithQueryRunner(
  id: ObjectId,
  queryRunner: QueryRunner,
): Promise<UserAccount> {
  const userAccount = await queryRunner.manager.findOneBy(UserAccount, {
    id: id,
  });
  if (!userAccount) {
    throw new UserAccountNotFoundError();
  }
  return userAccount;
}

async saveWithQueryRunner(
  userAccount: UserAccount,
  queryRunner: QueryRunner,
) {
  return queryRunner.manager.save(UserAccount, userAccount);
}

```

* 공통 로직(Transactional Decorator)

```typescript
export function Transactional(
  isolationLevel?: IsolationLevel,
): MethodDecorator {
  return (
    _target: any,
    _propertyKey: string,
    descriptor: PropertyDescriptor,
  ) => {
    const originalMethod = descriptor.value;

    descriptor.value = async function (...args: any[]) {
      const dataSource: DataSource = this.dataSource;
      if (!dataSource) {
        throw new Error('DataSource is not injected');
      }

      // 트랜잭션을 이미 사용하는 경우 추가로 트랜잭션을 생성하지 않는다.
      const existsTransaction = args.find(
        (arg) => arg.connection !== undefined,
      );
      if (existsTransaction) {
        return originalMethod.apply(this, args);
      }

      // 트랜잭션을 사용하지 않는 경우 트랜잭션을 생성한다.
      const queryRunner: QueryRunner = dataSource.createQueryRunner();
      await queryRunner.connect();
      await queryRunner.startTransaction(
        isolationLevel ? isolationLevel : DEFAULT_ISOLATION_LEVEL,
      );

      try {
        args = args.concat(queryRunner);
        const result = await originalMethod.apply(this, args);
        await queryRunner.commitTransaction();
        return result;
      } catch (err) {
        await queryRunner.rollbackTransaction();
        throw err;
      } finally {
        await queryRunner.release();
      }
    };
  };
}
```

* 결과

하나의 connect 에서 하나의 transactional 안에서 모든 로직이 실행한다.

<figure><img src="../../.gitbook/assets/Pasted image 20240114231327.png" alt=""><figcaption></figcaption></figure>

## 결론

NestJS와 TypeORM을 사용하여 트랜잭션 관리를 위한 코드 중복을 줄이고, 메서드의 책임을 분산하는 방법을 탐구했다.&#x20;

* **Transactional Decorator의 도입**
* **비즈니스 로직과 트랜잭션 관리의 분리**
* **하나의 Connect 내에서의 트랜잭션 실행**

결론적으로`Transactional` 데코레이터의 사용과 `QueryRunner`의 적절한 관리를 통해, NestJS 및 TypeORM 환경에서 트랜잭션을 효율적으로 관리할 수 있는 구조를 구축했다. 트랜잭션 관리에 대한 중복 코드를 줄이고, 비즈니스 로직의 명확성 및 유지보수성을 향상시켰다.

물론,이 데코레이터 사용하는 것이 어떤 추가적인 영향을 미칠지는 아직 불확실하다. 테스트코드를 작성하고 지속적으로 확인해서 개선 작업할 예정이다.

