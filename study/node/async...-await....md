# Async... Await...

## async function

* 자바스크립트는 기본적으로 동기적으로 실행되는 언어다.
  * 이는 코드가 작성된 순서대로 한 줄씩 실행되며, 각 라인의 코드가 완료되기 전까지는 다음 라인으로 넘어가지 않는다는 것을 의미한다.
  * 이러한 동기적 실행 모델은 코드의 흐름을 예측하기 쉽게 하주지만, 오래 걸리는 작업(예: 네트워크 요청)을 처리할 때 문제가 발생할 수 있다.
* `async`와 `await`은 자바스크립트에서 비동기 작업을 더 편리하게 다루기 위해 사용되는 키워드다.

### async

* 이 키워드는 비동기 함수를 선언하는 데 사용된다.
* `async` 함수는 `Promise` 를 반환한다.
* 서버에서 데이터를 가져오는 등의 시간이 걸리는 작업을 수행하는 함수가 있을 때 async를 선언하고 사용한다.
* 자바스크립트에게 이 함수를 비동기적으로 처리하라고 알려준다

```js
async function fetchData() {
	// API에서 데이터 가져오기
}
```

### await

* 이 키워드는 `async` 함수 내에서 `Promise` 가 해결될 때까지 기다리는 데 사용된다.
* 함수의 실행을 `Promise` 가 처리될 때까지(이행되거나 거부될 때까지) 일시 중지한다.
* `await` 는 오직 `async` 함수 내에서만 사용할 수 있다.

```js
async function fetchData() {
  let data = await fetch('https://api.example.com/data');
  let jsonData = await data.json();
  return jsonData;
}
```

### 예시

`await` 사용하는 경우

```js
function resolveAfter2Seconds() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('resolved');
      console.log('promise');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  // Expected output: 
  // > "calling" 
  // > "promise" 
  // > "resolved"
}

asyncCall();
```

`await` 사용하지 않는 경우

```js
function resolveAfter2Seconds() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('resolved');
      console.log('promise');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = resolveAfter2Seconds();
  console.log(result);
  // Expected output:
  // > "calling" 
  // > [object Promise] > 
  // "promise"`


asyncCall();
```

> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async\_function
