# Server-Sent Events (SSE)

## Short Polling ,Long Polling , SSE 비교

### Short Polling:

* **동작 방식**: 클라이언트가 일정한 간격으로 서버에 요청을 보내고 서버로부터 즉시 응답을 받습니다. 새로운 데이터가 없더라도 응답은 반환됩니다.
* **장점**: 구현이 간단하고 레거시 시스템과의 호환성이 뛰어납니다.
* **단점**: 새로운 데이터가 없을 때에도 요청과 응답이 지속되어 비효율적이고 서버 리소스 낭비가 발생할 수 있습니다.

#### 예시: (클라이언트는 주기적으로 이러한 요청을 반복합니다.)

* **클라이언트 요청:**

```http
GET /api/news HTTP/1.1
Host: server.example.com
```

* **서버 응답 (새 데이터가 없는 경우):**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{ "data": [] }
```

* **서버 응답 (새 데이터가 있는 경우):**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{ "data": [{"title": "New Article", "content": "Content of new article."}] }
```

### Long Polling:

* **동작 방식**: 클라이언트가 서버에 요청을 보내면 서버는 새로운 데이터가 있을 때까지 요청을 보류합니다. 새 데이터가 생기면 그때 응답을 보내고 클라이언트는 즉시 새 요청을 보냅니다.
* **장점**: 실시간에 가까운 통신이 가능하며 새 데이터가 없으면 요청을 보류하여 리소스를 절약할 수 있습니다.
* **단점**: 동시 연결 수가 많아질 경우 서버에 부담을 줄 수 있고, HTTP 연결 타임아웃 문제에 대한 처리가 필요합니다.

#### 예시:(클라이언트는 응답을 받은 후 즉시 새로운 요청을 보냅니다.)

* **클라이언트 요청:**

```http
GET /api/updates HTTP/1.1
Host: server.example.com
```

* **서버 응답 (새 데이터가 생길 때까지 대기 후 응답):**

<pre class="language-http"><code class="lang-http"><strong>HTTP/1.1 200 OK
</strong>Content-Type: application/json

{ "data": [{"title": "Updated Article", "content": "Updated content."}] }
</code></pre>

### Server-Sent Events (SSE):

* **동작 방식**: 클라이언트가 한 번 서버에 연결을 맺으면 서버는 새로운 데이터가 있을 때마다 클라이언트로 데이터를 보냅니다. 이는 단방향 통신 채널을 만듭니다.
* **장점**: 구현이 비교적 간단하며, HTTP 프로토콜을 사용하여 NAT와 방화벽 친화적입니다. 또한 자동 재연결 기능이 있습니다.
* **단점**: 브라우저 호환성 문제가 있을 수 있고, 브라우저가 아닌 클라이언트에서는 지원이 제한적일 수 있습니다. 이중 방향 통신이 필요한 경우 WebSocket을 고려해야 합니다.
* **`Accept: text/event-stream`** 헤더는 HTTP 요청에서 사용되며, 이는 클라이언트가 서버에서 전송되는 이벤트 스트림을 받을 준비가 되어 있다는 것을 나타냅니다

#### 예시:(클라이언트는 연결을 유지하고 서버로부터 데이터 스트림을 받습니다.)

* **클라이언트 요청 (서버에 SSE 연결 초기화):**

```http
GET /api/events HTTP/1.1
Host: server.example.com
Accept: text/event-stream
```

* **서버 응답 (연결을 유지하고 새 데이터가 생길 때마다 메시지 전송):**

```http
HTTP/1.1 200 OK
Content-Type: text/event-stream

data: {"title": "Breaking News", "content": "Details about breaking news."}

data: {"title": "Weather Update", "content": "Weather update details."}
```

### **총결론**:

* **Short Polling**: 심플한 상황이나 정기적인 데이터 업데이트가 필요하지 않은 경우 적합.
* **Long Polling**: 실시간 데이터를 처리해야 하지만, SSE나 WebSocket 같은 더 복잡한 기술을 사용하기 힘든 경우 적합.
* **SSE**: 실시간으로 서버에서 클라이언트로 데이터를 지속적으로 푸시해야 하는 경우 적합. 이중 방향 통신이 필요하지 않을 때 사용하면 좋습니다.

### 실시간 통신 방식에 대한 HTTP/2의 이점

* **Short Polling**:
  * Short Polling은 HTTP/2의 이점을 크게 활용하지 못할 수 있습니다. 다중 요청을 동시에 처리할 수 있는 능력이 향상되지만, 여전히 빈번한 연결 설정과 해제로 인한 비효율이 존재합니다.
* **Long Polling**:
  * HTTP/2의 다중화 기능으로 인해 여러 개의 Long Polling 연결이 동일한 TCP 연결 위에서 효율적으로 작동할 수 있으며, 헤더 압축으로 인해 오버헤드가 감소합니다.
* **Server-Sent Events (SSE)**:
  * SSE는 HTTP/2와 함께 사용될 때 더 효율적이 됩니다. 단일 연결을 통해 여러 개의 데이터 스트림을 전송할 수 있으며, 서버 푸시 기능을 통해 연관된 데이터를 클라이언트에 더 빠르게 전달할 수 있습니다.

### HTTP/2의 주요 이점

1. **다중화(Multiplexing)**:
   * 여러 요청과 응답이 동시에 같은 연결을 공유할 수 있으므로 여러 데이터 스트림이 경쟁하는 대신 효율적으로 관리됩니다.
   * 실시간 통신에서는 클라이언트가 여러 데이터 스트림을 개별적으로 받아 처리할 수 있어 효율적입니다.
2. **스트림 우선순위(Stream Prioritization)**:
   * 요청에 우선순위를 지정할 수 있어 중요한 요청을 먼저 처리할 수 있습니다.
   * 실시간 애플리케이션에서 중요한 메시지를 우선적으로 처리할 수 있는 이점이 있습니다.
3. **서버 푸시(Server Push)**:
   * 서버가 클라이언트의 요청을 기다리지 않고 예측 가능한 리소스를 미리 보낼 수 있습니다.
   * SSE와 같이 서버가 초기화하는 통신에 있어서 더 효율적이 될 수 있습니다.
4. **헤더 압축(Header Compression)**:
   * HPACK 압축을 사용하여 헤더 데이터를 압축함으로써 오버헤드를 감소시킵니다.
   * 모든 종류의 실시간 통신에서 반복되는 헤더 정보의 크기를 줄여 통신 효율을 개선합니다.
5. **보안 강화(TLS Enhancement)**:
   * HTTP/2는 TLS를 사용하는 보안 연결에 최적화되어 있으며, TLS 1.3과 함께 사용될 때 보안과 성능이 더욱 향상됩니다.
   * 실시간 데이터 전송의 보안을 강화하면서도 성능 저하를 최소화합니다.



