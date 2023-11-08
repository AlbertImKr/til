# SseEmitter (Spring)

Spring Framework에서 `SseEmitter`는 서버에서 클라이언트로 Server-Sent Events(SSE)를 보내기 위한 객체입니다. 서버에서 클라이언트로 실시간 업데이트를 전송하는 데 사용되는 서버 전송 이벤트(Server-Sent Events, SSE)를 보내기 위한 `ResponseBodyEmitter`의 특수 버전이다.

## 코드 요약

### **응답 확장**:

* `extendResponse(ServerHttpResponse outputMessage)`: `ResponseBodyEmitter`에서 오버라이드한 메서드로 SSE에 맞게 서버 응답을 커스터마이즈합니다. `Content-Type`을 `text/event-stream`으로 설정합니다(만약 아직 설정되지 않았다면).

### **이벤트 발송**:

* `send(Object object)`: 객체를 단일 SSE "data" 라인으로 전송합니다.
* `send(Object object, @Nullable MediaType mediaType)`: 지정된 `MediaType`을 가진 객체를 전송하여 메시지 컨버터 선택 시 컨텐츠 협상을 가능하게 합니다.
* `send(SseEventBuilder builder)`: id, 이벤트 이름, 코멘트 및/또는 재연결 시간과 데이터를 포함하는 복잡한 SSE 이벤트를 전송할 수 있습니다.

### **이벤트 빌딩**:

* `event()`: `SseEventBuilderImpl` 인스턴스를 반환하는 정적 메서드입니다. `SseEventBuilderImpl`은 내부 클래스로 `SseEventBuilder` 인터페이스를 구현합니다.

### **SseEventBuilder**:

* `SseEmitter` 클래스 내부의 인터페이스로, SSE 이벤트를 구축하기 위한 메서드를 정의합니다. 예를 들어 `id`, `name`, `reconnectTime`, `comment`, `data` 등의 메서드가 있습니다.

### **SseEventBuilderImpl**:

* `SseEventBuilder` 인터페이스를 구현하는 내부 private static 클래스입니다. SSE 형식의 문자열을 구성하기 위해 `StringBuilder`를 사용하고 전송할 데이터를 담는 `Set<DataWithMediaType>`를 사용합니다.

### **DataWithMediaType**:

* 전송될 객체와 그 `MediaType`을 연관시키는 간단한 컨테이너 클래스로 보입니다. 내부적으로 `SseEventBuilderImpl`에 의해 사용됩니다.

`send(SseEventBuilder builder)` 메서드에서 `synchronized`를 사용하는 것은 데이터가 **스레드 안전**한 방식으로 전송되도록 보장합니다. 서버 전송 이벤트는 일반적으로 서버가 실시간으로 업데이트를 클라이언트 브라우저로 **푸시**해야 하는 애플리케이션, 예를 들어 대시보드 또는 **실시간**으로 업데이트되는 피드에서 사용됩니다.

## 예시

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.http.MediaType;
import org.springframework.web.servlet.mvc.method.annotation.SseEmitter;

@Controller
@RequestMapping("/sse")
public class SseController {

    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamSseMvc() {
        SseEmitter emitter = new SseEmitter();
        // 새로운 스레드에서 비동기로 처리
        Executors.newSingleThreadExecutor().execute(() -> {
            try {
                for (int i = 0; i < 10; i++) {
                    // 데이터를 전송하기 전에 서버에서 데이터를 생성하거나 검색합니다.
                    Thread.sleep(1000);
                    emitter.send("/sse/stream - " + LocalTime.now().toString());
                }
                emitter.complete();
            } catch (IOException | InterruptedException e) {
                emitter.completeWithError(e);
            }
        });
        return emitter;
    }
}
```

