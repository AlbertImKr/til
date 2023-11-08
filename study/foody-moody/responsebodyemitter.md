# ResponseBodyEmitter

`ResponseBodyEmitter`는 Spring MVC에서 사용되는 클래스로, 비동기 요청 처리에서 하나 이상의 객체를 응답에 쓸 때 사용됩니다.

이 클래스는 컨트롤러 메소드의 반환 타입으로 사용되며, 단일 결과를 생성하는 `DeferredResult`와는 달리 여러 객체를 보낼 수 있게 해주는데, 각 객체는 호환되는 `HttpMessageConverter`를 사용하여 작성됩니다.

### 주요 기능 및 특징

* **Asynchronous Processing**: `ResponseBodyEmitter`를 사용하면 데이터를 비동기적으로 스트리밍할 수 있으며, 한 번에 전체 응답을 보내는 대신에 시간이 지남에 따라 데이터를 여러 조각으로 보낼 수 있습니다.
* **Multiple Sends**: 여러 개의 `send` 호출을 통해 시간이 지남에 따라 데이터 조각을 보낼 수 있습니다. 이는 클라이언트에게 실시간 업데이트를 제공하는 데 유용합니다.
* **Timeout Management**: 생성자에 타임아웃 값을 설정하여 `ResponseBodyEmitter`의 타임아웃을 제어할 수 있습니다.
* **Error Handling**: `completeWithError` 메소드를 통해 에러가 발생했을 때의 처리를 정의할 수 있습니다.
* **Completion Callbacks**: `onCompletion`, `onTimeout`, `onError` 메소드를 통해 해당 이벤트가 발생했을 때 호출될 콜백을 등록할 수 있습니다.

요청을 처리하는 도중에 언제든지 `emitter.send(foo)`를 호출하여 클라이언트에 데이터를 전송할 수 있고, 데이터 전송이 끝나면 `emitter.complete()`를 호출하여 요청 처리를 완료할 수 있습니다.

### 예시

```java
  @RequestMapping(value="/stream", method=RequestMethod.GET)
  public ResponseBodyEmitter handle() {
  	   ResponseBodyEmitter emitter = new ResponseBodyEmitter();
  	   // Pass the emitter to another component...
  	   return emitter;
  }
 
  // in another thread
  emitter.send(foo1);
 
  // and again
  emitter.send(foo2);
 
  // and done
  emitter.complete();
```
