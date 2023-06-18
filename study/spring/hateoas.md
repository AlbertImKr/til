---
description: Hypermedia as the engine of application state
---

# HATEOAS

Hypermedia as the engine of application state (HATEOAS) is a constraint of the REST application architecture that distinguishes it from other network application architectures.

* Hypermedia,
  * an extension of the term hypertext, is a nonlinear medium of information that includes graphics, audio, video, plain text and hyperlinks.
* Hyperlink,
  * In computing, a hyperlink, or simply a link, is a digital reference to data that the user can follow or be guided to by clicking or tapping

## Example

user-agent는 진입점 URL을 통해 REST API에 HTTP 요청을 합니다.

user-agent가 할 수 있는 모든 후속 요청은 각 요청에 대한 응답 내에서 발견됩니다.

### Requset

```http
GET /accounts/12345 HTTP/1.1
Host: bank.example.com
```

### Response

```json
HTTP/1.1 200 OK

{
    "account": {
        "account_number": 12345,
        "balance": {
            "currency": "usd",
            "value": 100.00
        },
        "links": {
            "deposits": "/accounts/12345/deposits",
            "withdrawals": "/accounts/12345/withdrawals",
            "transfers": "/accounts/12345/transfers",
            "close-requests": "/accounts/12345/close-requests"
        }
    }
}
```

### CODE

#### HATEOAS 적용전

```java
@GetMapping
public ResponseEntity<Page<Product>> findAll(Principal principal,
        Pageable pageable) {
    Page<Product> products =
            productDao.findProductsByUserEmail(principal.getName(), pageable);
    return ResponseEntity.ok(products);
}
```

```json
HTTP/1.1 200 OK
Content-Type: application/hal+json
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Length: 9362

{
  "_embedded" : {
    "productResponseList" : [ {
      "productId" : "648dcfb047d916749c554cd4",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
    }, {
      "productId" : "648dcfb047d916749c554cd5",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
    }
   ........
   ........
   ........
    , {
      "productId" : "648dcfb047d916749c554ce7",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
    } ]
  },
  "page" : {
    "size" : 20,
    "totalElements" : 100,
    "totalPages" : 5,
    "number" : 1
  }
}
```



#### HATEOAS 적용후

```java
@GetMapping
public ResponseEntity<PagedModel<ProductResponse>> findAll(Principal principal,
    Pageable pageable) {
Page<Product> products =
        productDao.findProductsByUserEmail(principal.getName(), pageable);

PagedModel<ProductResponse> productResponses = pagedResourcesAssembler
        .toModel(products, productAssembler);

return ResponseEntity.ok(productResponses);
}

@RequiredArgsConstructor
@RestController
@RequestMapping(path = "/products", produces = MediaTypes.HAL_JSON_VALUE)
public class ProductController {

    private final ProductService productService;
    private final StoreDao storeDao;
    private final ProductDao productDao;
    private final PagedResourcesAssembler<Product> pagedResourcesAssembler;
    private final ProductAssembler productAssembler;

    @PostMapping
    public ResponseEntity<CreatedProductResponse> addProduct(
            @RequestBody ProductRequest productRequest,
            Principal principal) {
        StoreId storeId = storeDao.findStoreIdByUserEmail(principal.getName());
        CreatedProductResponse productResponse = productService.addProduct(productRequest, storeId);

        ProductId productId = productResponse.getProductId();
        Link selfRel = BusinessLinks.getProductSelfRel(productId);
        productResponse.add(selfRel, BusinessLinks.MY_STORE);
        return ResponseEntity.created(selfRel.toUri()).body(productResponse);
    }


    @GetMapping
    public ResponseEntity<PagedModel<ProductResponse>> findAll(Principal principal,
            Pageable pageable) {
        Page<Product> products =
                productDao.findProductsByUserEmail(principal.getName(), pageable);

        PagedModel<ProductResponse> productResponses = pagedResourcesAssembler
                .toModel(products, productAssembler);

        return ResponseEntity.ok(productResponses);
    }
}

```

```json
HTTP/1.1 200 OK
Content-Type: application/hal+json
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Length: 9362

{
  "_embedded" : {
    "productResponseList" : [ {
      "productId" : "648dcfb047d916749c554cd4",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/products/648dcfb047d916749c554cd4"
        }
      }
    }, {
      "productId" : "648dcfb047d916749c554cd5",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/products/648dcfb047d916749c554cd5"
        }
      }
    }
    ........
    ........
    ........
    , {
      "productId" : "648dcfb047d916749c554ce7",
      "productName" : "testProductName",
      "price" : 10000,
      "description" : "testDescription",
      "brand" : "testBrand",
      "category" : "testCategory",
      "createdTime" : "20230618002224",
      "updateTime" : "20230618002224",
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/products/648dcfb047d916749c554ce7"
        }
      }
    } ]
  },
  "_links" : {
    "first" : {
      "href" : "http://localhost:8080/products?page=0&size=20"
    },
    "prev" : {
      "href" : "http://localhost:8080/products?page=0&size=20"
    },
    "self" : {
      "href" : "http://localhost:8080/products?page=1&size=20"
    },
    "next" : {
      "href" : "http://localhost:8080/products?page=2&size=20"
    },
    "last" : {
      "href" : "http://localhost:8080/products?page=4&size=20"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 100,
    "totalPages" : 5,
    "number" : 1
  }
}
```

## 장단점

### 장점

#### 클라이언트-서버 독립성

* HATEOAS는 클라이언트와 서버 간의 강력한 결합을 피하고 클라이언트에 대한 API 변경의 여향을 최소화합니다. 클라이언트는 하이퍼미디어 링크를 통해 리소스와 상호작용하고, 서버에서 제공하는 리소스 표현에 대해 사전 지식이 필요하지 않습니다. 이로써 서버 측의 변경에도 클라이언트가 유연하게 대응할 수 있습니다.

#### API탐색 및 디스커버리

* HATEOA는 클라이언트에게 API의 구조와 사용 가능한 작업을 동적으로 제공합니다. 클라이언트는 시작점 리소스로부터 출발하여 하이퍼미디어 링크를 따라가며 서버가 제공하는 리소스와 상호작용할 수 있습니다. 이는 API를 탐색하고 발견하는 데 도움을 주며, 클라이언트 코드에서 하드 코딩된 엔드포인트를 제거하고 유연성과 화장석을 높을 수 있습니다.

#### 셀프 설명형 API

* HATEOAS를 사용하면 하이퍼미디어 링크와 함께 리소스 표현이 제공됩니다. 이를 통해 API가 자기 설명적(self-descriptive)이 되어 클라이언트가 리소스의 의미와 상호작용 방법을 이해 할 수 있습니다. 따라서 클라이언트 개발자가 API문서를 읽고 해석하는 시간과 노력을 줄일 수 있습니다.

### 단점

#### 복잡성

* HATEOA를 구현하려면 API 설계와 클라이언트 코드를 더 복잡하게 만들어야 합니다. 클라이언트는 하이퍼미디어 링크를 이해하고 해석하는 노리를 추가로 구현해야 하며, 서버느 링크를 동적으로 생성하고 관리해야 한다. 이는 개발 및 유지보수 비용을 증가시킬 수 있습니다.

#### 네트워크 오버헤드

* HATEOA를 사용하면 많은 하이퍼미디어 링크가 리소스 표현에 포함될 수 있습니다. 이는 API 응답의 크기를 증가시킬수 있고, 네트워크 전송 시간과 대역폭을 소비할 수 있습니다. 따라서 대량의 데이터를 처리하는 경우에는 성능 문제가 발생할 수 있습니다.

#### 클라이언트 구현의 어려움

* HATEOAS를 지원하지 않는 클라이언트는 링크와 리소스 상호작용을 제대로 처리할 수 없습니다. 클라이언트 측에 HATEOAS를 지원하는 라이브러리나 프레임워크를 도입해야 하며, 기존의 클라이언트를 수정해야 할 수도 있습니다. 이는 일부 개발자에게 추가 작업으로 다가올 수 있습니다.





## Reference

{% embed url="https://en.wikipedia.org/wiki/HATEOAS" %}
