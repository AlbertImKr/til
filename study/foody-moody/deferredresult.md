# DeferredResult

이 클래스는 비동기 처리를 위해 사용되며, 결과가 바로 준비되지 않을 때 사용합니다. 클라이언트 요청은 바로 반환되고, 실제 결과는 나중에 설정될 수 있습니다.

## **필드**:

* `timeoutValue`: 타임아웃을 위한 값입니다. `null`이 될 수도 있고, 밀리초 단위로 타임아웃 시간을 나타냅니다.
* `timeoutResult`: 타임아웃 발생 시 반환할 기본 결과를 제공하는 `Supplier` 객체입니다.
* 콜백 함수를 위한 필드들이 여러 개 있습니다: 타임아웃(`timeoutCallback`), 에러(`errorCallback`), 완료(`completionCallback`) 시 호출됩니다.
* `resultHandler`: 결과를 처리할 때 사용하는 핸들러입니다.
* `result`: 결과를 저장하는 변수입니다. 초기 상태는 `RESULT_NONE`으로 설정됩니다.
* `expired`: 객체가 만료되었는지를 나타내는 플래그입니다.

## **메소드**:

* 결과가 설정되었는지 확인하는 `isSetOrExpired()`
* 결과를 가져오는 `getResult()`
* 결과를 설정하는 `setResult()`와 `setErrorResult()`
* 각종 콜백을 설정하는 메소드들: `onTimeout()`, `onError()`, `onCompletion()`
* 결과를 처리할 `DeferredResultHandler` 인터페이스를 정의합니다.

이 클래스는 일반적으로 웹 요청을 처리할 때 사용됩니다. 요청이 들어오면 서버는 `DeferredResult` 객체를 생성하고 클라이언트에게 바로 반환합니다. 서버는 다른 스레드에서 계산이나 처리를 계속하고, 결과가 준비되면 `DeferredResult` 객체에 결과를 설정합니다. 그러면 스프링 프레임워크가 클라이언트에게 결과를 자동으로 보냅니다.

## 예시

* 먼저, 서비스 계층의 어떤 기능을 비동기적으로 실행하기 위한 설정

```java
@Service
public class SomeAsyncService {
    @Async
    public CompletableFuture<String> performLongRunningOperation() {
        // 여기서 긴 처리를 수행한다고 가정합니다. 예를 들어, 외부 API 호출을 할 수 있습니다.
        try {
            Thread.sleep(5000); // 5초간 대기를 나타냅니다. 실제로는 복잡한 처리를 수행할 것입니다.
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        return CompletableFuture.completedFuture("결과가 준비되었습니다");
    }
}
```

그리고 컨트롤러에서 이 서비스를 비동기적으로 호출하는 방법

<pre class="language-java"><code class="lang-java">@RestController
public class SomeController {
    private final SomeAsyncService someAsyncService;

    public SomeController(SomeAsyncService someAsyncService) {
        this.someAsyncService = someAsyncService;
    }

    @GetMapping("/asyncOperation")
    public DeferredResult&#x3C;String> executeAsync() {
        DeferredResult&#x3C;String> deferredResult = new DeferredResult&#x3C;>();

<strong>        someAsyncService.performLongRunningOperation().thenAccept(result -> {
</strong>            deferredResult.setResult(result);
        }).exceptionally(ex -> {
            deferredResult.setErrorResult("오류가 발생했습니다: " + ex.getMessage());
            return null;
        });

        return deferredResult;
    }
}
</code></pre>

## `DeferredResult`는 다음과 같은 경우에 유용하다

* 외부 시스템으로부터의 비동기적인 응답을 기다릴 때
* 결과가 준비되는 데 시간이 오래 걸리는 작업을 처리할 때
* 스프링 MVC의 스레드를 불필요하게 점유하지 않고 다른 요청을 처리하게 하고 싶을 때
