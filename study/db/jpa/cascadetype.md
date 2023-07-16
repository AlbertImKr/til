---
description: 연쇄 작업 유형
---

# CascadeType

## 유형

### `CascadeType.ALL`

모든 연쇄 작업을 수행합니다. 저장(PERSIST), 병합(MERGE), 삭제(REMOVE), 새로고침(REFRESH), 및 분리(DETACH) 작업이 연관된 엔티티에 적용됩니다.&#x20;

즉, 부모 엔티티에 적용된 모든 작업이 연관된 자식 엔티티에도 전파됩니다. 따라서 부모 엔티티의 저장, 병합, 삭제, 새로고침, 분리 작업이 연관된 엔티티에 모두 적용됩니다.

### `CascadeType.PERSIST`

저장 작업을 수행합니다. 연관된 엔티티가 새로 생성되거나 기존에 있던 경우에도 저장 작업이 연관된 엔티티에 적용됩니다.

따라서 연관된 엔티티를 새로 생성하거나 이미 존재하는 경우에도 저장 작업이 적용됩니다.

```java
ParentEntity parent = new ParentEntity();
parent.getChildren().add(new ChildEntity());  // 새로운 자식 엔티티 추가

entityManager.persist(parent);  // 부모와 연관된 자식 엔티티들이 모두 저장됨
```

### `CascadeType.MERGE`

병합 작업을 수행합니다. 연관된 엔티티의 변경 내용이 병합 작업을 수행한 엔티티에 적용됩니다.

즉, 연관된 엔티티의 변경 내용이 병합 작업을 수행한 엔티티에 적용됩니다.

```java
ParentEntity parent = entityManager.find(ParentEntity.class, parentId);
parent.getChildren().get(0).setName("New Name");  // 자식 엔티티의 속성 변경

entityManager.merge(parent);  // 변경된 자식 엔티티가 병합됨
```

### `CascadeType.REMOVE`

삭제 작업을 수행합니다. 연관된 엔티티가 삭제될 때 해당 작업이 연관된 엔티티에 적용됩니다.

연관된 엔티티가 삭제될 때 해당 작업이 연관된 엔티티에 적용됩니다.

```java
ParentEntity parent = entityManager.find(ParentEntity.class, parentId);
parent.getChildren().remove(0);  // 자식 엔티티 제거

entityManager.merge(parent);  // 연관된 자식 엔티티가 삭제됨
```

### `CascadeType.REFRESH`

새로고침 작업을 수행합니다. 연관된 엔티티가 새로고침될 때 해당 작업이 연관된 엔티티에 적용됩니다.

연관된 엔티티의 상태가 변경되어도 이 작업을 사용하여 새로운 상태로 엔티티를 업데이트할 수 있습니다.

```java
ParentEntity parent = entityManager.find(ParentEntity.class, parentId);
parent.getChildren().get(0).setName("Updated Name");  // 자식 엔티티의 속성 변경

entityManager.refresh(parent);  // 부모와 연관된 자식 엔티티가 모두 새로고침됨
```

### `CascadeType.DETACH`

분리 작업을 수행합니다. 연관된 엔티티가 분리될 때 해당 작업이 연관된 엔티티에 적용됩니다.

분리 작업을 수행하면 연관된 엔티티가 영속성 컨텍스트에서 분리되어 변경 사항을 추적하지 않게 됩니다.

```java
ParentEntity parent = entityManager.find(ParentEntity.class, parentId);

entityManager.detach(parent);  // 부모와 연관된 자식 엔티티들이 모두 분리됨
```

## 가이드라인

`CascadeType`를 선택할 때에는 관련된 엔티티 간의 의존성, 비즈니스 규칙, 데이터 일관성 요구사항을 고려하고, 실제로 필요한 연쇄 작업을 정확하게 지정해야 합니다. 적절한 `CascadeType`을 선택하면 연관된 엔티티 간의 작업을 편리하게 처리하고 데이터 일관성을 유지할 수 있습니다.

### 저장 작업 (CascadeType.PERSIST):

* 부모 엔티티와 연관된 자식 엔티티를 항상 함께 저장해야 하는 경우에 유용합니다.
* 자식 엔티티를 부모 엔티티에 추가하고, 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장됩니다.
* 예를 들어, 부모-자식 관계에서 부모를 저장할 때 자식도 함께 저장되어야 하는 경우 사용할 수 있습니다.

### 병합 작업 (CascadeType.MERGE):

* 부모 엔티티와 연관된 자식 엔티티의 변경 내용을 부모 엔티티와 함께 병합해야 하는 경우에 유용합니다.
* 자식 엔티티를 수정하고, 부모 엔티티를 병합할 때 자식 엔티티의 변경 내용도 함께 병합됩니다.
* 예를 들어, 부모-자식 관계에서 부모를 병합할 때 자식의 변경 내용도 함께 병합되어야 하는 경우 사용할 수 있습니다.

### 삭제 작업 (CascadeType.REMOVE):

* 부모 엔티티가 삭제될 때 연관된 자식 엔티티도 함께 삭제해야 하는 경우에 유용합니다.
* 부모 엔티티를 삭제하면 연관된 자식 엔티티도 자동으로 삭제됩니다.
* 예를 들어, 부모-자식 관계에서 부모를 삭제할 때 자식도 함께 삭제되어야 하는 경우 사용할 수 있습니다.

### 새로고침 작업 (CascadeType.REFRESH):

* 부모 엔티티를 새로고침할 때 연관된 자식 엔티티도 함께 새로고침해야 하는 경우에 유용합니다.
* 부모 엔티티를 새로고침하면 연관된 자식 엔티티도 자동으로 새로고침됩니다.
* 예를 들어, 부모-자식 관계에서 부모를 새로고침할 때 자식도 함께 새로고침되어야 하는 경우 사용할 수 있습니다.

### 분리 작업 (CascadeType.DETACH):

* 부모 엔티티를 분리할 때 연관된 자식 엔티티도 함께 분리해야 하는 경우에 유용합니다.
* 부모 엔티티를 분리하면 연관된 자식 엔티티도 자동으로 분리됩니다.
* 예를 들어, 부모-자식 관계에서 부모를 분리할 때 자식도 함께 분리되어야 하는 경우 사용할 수 있습니다.
