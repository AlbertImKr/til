# NO-CACHE(HTTP)

## **Cache-Control 지시어**

* **`max-age=<seconds>`**:
  * 리소스가 캐시될 수 있는 최대 시간을 초 단위로 지정합니다.
* **`no-cache`**:
  * 매번 원본 서버로부터 리소스의 유효성을 검증해야 합니다.
* **`no-store`**:
  * 리소스를 캐시에 저장하면 안 됩니다.
* **`private`**:
  * 응답은 특정 사용자를 위한 것이므로 중간 캐시에 저장하면 안 됩니다.
* **`public`**:
  * 응답은 어떠한 캐시에서도 저장될 수 있습니다.
* **`must-revalidate`**:
  * 캐시의 리소스가 만료된 후에는 원본 서버로부터 유효성 검증을 받지 않고는 사용되어서는 안 됩니다.

## NO-CACHE ❗❗

* `no-cache`는 리소스를 캐시하지 않으라는 의미가 아니다. ❗❗❗
* 대신 이 지시어는 클라이언트나 중간 캐시(예: 프록시 서버)가 캐시된 리소스를 사용하기 전에 항상 원본 서버에서 리소스의 유효성을 검증한다. ❓ ❓ ❓
* **`no-cache`** 지시어가 설정되면 클라이언트나 중간 캐시는 원본 서버에 요청을 보내고 \*\*`ETag`\*\*나 **`Last-Modified`** 값과 같은 유효성 검사 헤더를 사용하여 현재 캐시된 리소스가 여전히 유효한지 확인합니다. ❓ ❓ ❓

\<aside> 😓 한번도 사용해본적 없는데❓❓ 어떻게 사용하지❓❓

\</aside>

## 처리 순서(예시)

1. **클라이언트 요청**:
   * 클라이언트가 특정 리소스를 요청합니다.
2. **캐시 확인**:
   * 클라이언트의 브라우저나 중간 캐시(프록시 서버 등)는 해당 리소스가 이미 캐시되어 있는지 확인합니다.
3. **캐시된 리소스의 유효성 검사**:
   * 리소스가 캐시되어 있다면, 클라이언트나 중간 캐시는 원본 서버에게 해당 리소스의 유효성을 검사하라는 요청을 보냅니다.
   * 이 요청은 \*\*`ETag`\*\*나 **`Last-Modified`** 헤더를 포함하여 리소스의 마지막 상태를 서버에 전달합니다.
4. **서버의 응답**:
   * 원본 서버는 클라이언트로부터 받은 \*\*`ETag`\*\*나 **`Last-Modified`** 값을 자신이 가진 최신 리소스의 값과 비교합니다.
     * **변경되지 않은 경우**: 서버는 **`304 Not Modified`** 응답을 전송합니다. 이 경우, 클라이언트나 중간 캐시는 자신이 가진 캐시된 리소스를 사용합니다.
     * **변경된 경우**: 서버는 변경된 리소스와 함께 **`200 OK`** 응답을 전송합니다. 클라이언트나 중간 캐시는 이 리소스를 캐시에 저장하고 사용자에게 제공합니다.
5.  **리소스의 전달**:

    * 클라이언트는 서버의 응답을 바탕으로 캐시된 리소스를 사용하거나 새로 받은 리소스를 사용하여 요청한 페이지나 컨텐츠를 표시합니다.

    \<aside> 🧐 **`ETag`** (Entity Tag)는 HTTP 응답 헤더의 일종으로, 웹 서버가 리소스의 특정 버전 또는 상태를 나타내기 위해 제공하는 고유한 식별자입니다. \*\*`ETag`\*\*의 주요 목적은 클라이언트와 서버 간에 특정 리소스의 변경 여부를 효율적으로 확인하는 것입니다.

    \</aside>

## \*\*`ETag`\*\*나 **`Last-Modified`** 헤더를 포함하여 리소스의 마지막 상태를 서버에 어떻게 전달 할까 ❓

### HTTP(예시)

1.  **첫 번째 요청** (클라이언트에서 서버로):

    ```yaml
    GET /resource.jpg HTTP/1.1
    Host: example.com
    ```
2.  **첫 번째 응답** (서버에서 클라이언트로):

    ```yaml
    HTTP/1.1 200 OK
    Content-Type: image/jpeg
    Cache-Control: no-cache
    ETag: "abcdef12345"
    Last-Modified: Wed, 15 Oct 2020 07:26:05 GMT
    ...

    [리소스 데이터]
    ```
3.  **재요청** (클라이언트에서 서버로, 이전 응답에서 받은 \*\*`ETag`\*\*와 **`Last-Modified`** 값을 활용):

    ```yaml
    GET /resource.jpg HTTP/1.1
    Host: example.com
    If-None-Match: "abcdef12345"
    If-Modified-Since: Wed, 15 Oct 2020 07:26:05 GMT
    ```
4.  **두 번째 응답** (두 가지 시나리오가 있습니다):

    * 리소스가 변경되지 않았을 때 (서버에서 클라이언트로):

    ```yaml
    HTTP/1.1 304 Not Modified
    Cache-Control: no-cache
    ETag: "abcdef12345"
    Last-Modified: Wed, 15 Oct 2020 07:26:05 GMT
    ```

    * 리소스가 변경되었을 때 (서버에서 클라이언트로):

    ```yaml
    HTTP/1.1 200 OK
    Content-Type: image/jpeg
    Cache-Control: no-cache
    ETag: "newEtagValue67890"
    Last-Modified: Fri, 17 Oct 2020 10:15:30 GMT
    ...

    [새로운 리소스 데이터]
    ```

## 장단점 ❓

### **장점:**

#### 1. **효율성과 성능 향상**:

* 클라이언트는 변경되지 않은 리소스를 다시 다운로드할 필요 없이 로컬 캐시에서 리소스를 읽을 수 있습니다. 이로 인해 대역폭 사용이 줄어들고 웹 페이지의 로딩 시간이 단축됩니다.

#### 2. **유연성**:

* \*\*`ETag`\*\*와 \*\*`Last-Modified`\*\*를 사용하면, 리소스의 실제 변경 여부를 기반으로 캐싱 결정을 할 수 있습니다. 리소스의 내용이 변경되지 않았다면, 서버는 **`304 Not Modified`** 응답을 반환하여 리소스 다운로드를 방지할 수 있습니다.

#### 3. **최신 상태 유지**:

* **`no-cache`** 디렉티브를 사용하면 클라이언트는 리소스의 상태를 항상 최신으로 유지할 수 있습니다. 이는 특히 빈번하게 변경되는 리소스에서 중요합니다.

### **단점:**

#### 1. **추가 요청 오버헤드**:

* **`no-cache`** 디렉티브를 사용하면 클라이언트는 리소스의 상태를 확인하기 위해 매번 서버에 요청을 보내야 합니다. 이로 인해 추가적인 요청 오버헤드가 발생할 수 있습니다.

#### 2. **서버 부하 증가**:

* 매번 리소스의 유효성을 검증하기 위한 요청이 서버에 도달하면, 서버의 부하가 증가할 수 있습니다.

#### 3. **캐시 계산 오버헤드**:

* **`ETag`** 값의 생성에는 약간의 오버헤드가 있을 수 있습니다, 특히 내용 기반의 **`ETag`** 값을 생성하는 경우에 그렇습니다.

#### 4. **타이밍 이슈**:

* \*\*`Last-Modified`\*\*는 초 단위로만 작동하기 때문에, 같은 초 내에 여러 번 수정된 리소스에 대한 정확한 변경 감지가 어려울 수 있습니다.

## 그럼 언제 사용해야 하나요 ❓

### **ETag**

#### 1. **빈번하게 변경되는 리소스**:

* 리소스가 자주 변경되거나 그 변경 패턴이 예측 불가능할 때, \*\*`ETag`\*\*를 사용하여 정확한 버전을 추적할 수 있습니다.

#### 2. **정밀한 유효성 검사가 필요할 때**:

* \*\*`ETag`\*\*는 리소스의 내용 기반으로 생성되므로 리소스의 실제 내용 변경을 감지하는 데 사용될 수 있습니다.

#### 3. **밀리초 단위의 정확성이 필요할 때**:

* \*\*`Last-Modified`\*\*는 초 단위로만 작동하지만, \*\*`ETag`\*\*는 내용의 미세한 변경을 감지할 수 있습니다.

### **Last-Modified**

#### 1. **정기적으로 업데이트되는 리소스**:

* 리소스의 변경 패턴이 예측 가능하고 초 단위의 정밀성이 충분할 때 사용됩니다.

#### 2. **서버 오버헤드 감소**:

* \*\*`ETag`\*\*보다 \*\*`Last-Modified`\*\*의 생성 및 검증 오버헤드가 일반적으로 더 적습니다.

### **Cache-Control: no-cache**

#### 1. **항상 최신 상태를 유지해야 하는 리소스**:

* 예를 들어, 주식 가격, 뉴스 피드, 실시간 스포츠 점수와 같이 계속 업데이트되는 정보를 포함하는 리소스에 적합합니다.

#### 2. **캐시는 원하지만 유효성 검사도 필요한 경우**:

* 리소스를 캐시하되, 사용하기 전에 항상 유효성을 검사하려는 경우에 적합합니다.

#### 3. **중간 프록시에서 캐싱 방지**:

* **`no-cache`** 디렉티브는 중간 프록시에서의 캐싱을 방지하고, 클라이언트가 항상 원래의 서버와의 통신을 유지하도록 합니다.

## React (프론트엔드)와 Java Spring (백엔드)이면 어떻게 구현해야 할까❓

### 1. **첫 번째 요청 (React 클라이언트에서 Java Spring 서버로):**

```jsx
fetch("<https://example.com/api/data>", {
  method: "GET",
  headers: {
    "Accept": "application/json"
  }
})
.then(response => response.json())
.then(data => {
  // ... data 처리 ...
});
```

### **2. 첫 번째 응답 (Java Spring 서버에서 React 클라이언트로):**

```java
@RestController
@RequestMapping("/api")
public class DataController {
  
  @GetMapping("/data")
  public ResponseEntity<Data> getData() {
    Data data = ...; // 데이터 로직
    String eTagValue = ...; // ETag를 계산 (예: MD5, SHA-1)

    return ResponseEntity.ok()
      .cacheControl(CacheControl.noCache())
      .eTag(eTagValue)
      .body(data);
  }
}
```

#### 응답 헤더:

```java
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-cache
ETag: "abcdef12345"
...

{ ... 데이터 ... }
```

### **3. 재요청 (React 클라이언트에서 Java Spring 서버로):**

```jsx
fetch("<https://example.com/api/data>", {
  method: "GET",
  headers: {
    "Accept": "application/json",
    "If-None-Match": "abcdef12345"  // 이전 응답에서 받은 ETag 값
  }
})
.then(response => {
  if (response.status === 304) {
    // ... 캐시된 데이터 사용 ...
  } else {
    return response.json();
  }
})
.then(data => {
  // ... 새로운 데이터 처리 (if applicable) ...
});
```

### **4. 두 번째 응답 (Java Spring 서버에서 React 클라이언트로):**

```java
@GetMapping("/data")
public ResponseEntity<Data> getData(@RequestHeader(value = "If-None-Match", required = false) String eTag) {
  String currentETagValue = ...; // 현재 ETag 계산

  if (currentETagValue.equals(eTag)) {
    return ResponseEntity.status(HttpStatus.NOT_MODIFIED).build();
  }

  Data newData = ...; // 데이터 로직

  return ResponseEntity.ok()
    .cacheControl(CacheControl.noCache())
    .eTag(currentETagValue)
    .body(newData);
}
```

#### **응답 헤더** (리소스가 변경되지 않았을 경우):

```java
HTTP/1.1 304 Not Modified
Cache-Control: no-cache
ETag: "abcdef12345"
```

#### **응답 헤더** (리소스가 변경되었을 경우):

```java
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-cache
ETag: "newEtagValue67890"
...

{ ... 새로운 데이터 ... }
```

## 여기 까지 나의 궁금증 해결됐어요. 다음에 한번 적용하고 사용해보겠습니다. 😊
