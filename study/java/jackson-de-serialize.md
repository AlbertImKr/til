---
description: Jackson 역직렬
---

# Jackson de/serialize

Jackson은 Java 객체와 JSON 데이터 간의 직렬화 및 역직렬화를 수행하는 데 사용되는 강력한 라이브러리입니다. `ObjectMapper`를 사용하여 객체를 JSON으로 직렬화하고, JSON을 객체로 역직렬화할 수 있습니다

## 직렬화(Serialization): Java 객체를 JSON 형식으로 변환합니다.

* `ObjectMapper` 클래스를 사용하여 Java 객체를 JSON 문자열로 직렬화할 수 있습니다.
* `writeValue()` 메서드를 사용하여 Java 객체를 JSON으로 변환할 수 있습니다.
* `writeValueAsString()` 메서드를 사용하여 Java 객체를 JSON 문자열로 변환할 수 있습니다.

## 역직렬화(Deserialization):JSON 데이터를 Java 객체로 변환합니다.

* `ObjectMapper` 클래스를 사용하여 JSON 문자열을 Java 객체로 역직렬화할 수 있습니다.
* `readValue()` 메서드를 사용하여 JSON 문자열을 Java 객체로 변환할 수 있습니다.
* `readValue()` 메서드는 다양한 입력 형식을 지원하며, JSON 문자열, 파일, URL, InputStream 등을 처리할 수 있습니다.

## 역질력 기본 생성자 필수

기본적으로 Jackson은 객체를 역직렬화할 때, 기본 생성자를 사용하여 객체를 인스턴스화합니다. 기본 생성자가 없는 경우, Jackson은 객체를 역직렬화할 수 없으며, `JsonMappingException` 예외가 발생합니다.

기본 생성자의 필요성은 역직렬화에만 해당합니다. 직렬화 시에는 기본 생성자가 반드시 필요하지는 않습니다. 직렬화는 Java 객체를 JSON 형식으로 변환하는 과정이므로, 객체의 상태를 기록하고 필드 값을 JSON으로 변환합니다. 이때 생성자는 호출되지 않습니다.

하지만 역직렬화 시, JSON 데이터를 Java 객체로 변환할 때, 해당 클래스의 기본 생성자가 필요합니다. 역직렬화 과정에서 Jackson은 JSON 데이터를 기반으로 객체를 인스턴스화하고, 필드 값을 설정합니다. 기본 생성자가 없는 경우, Jackson은 객체를 인스턴스화할 수 없으며, 역직렬화 시에 예외가 발생합니다.

따라서 Jackson을 사용하여 역직렬화를 수행할 때는 해당 클래스에 기본 생성자가 필요합니다. 직렬화 시에는 기본 생성자가 없어도 문제가 없습니다.

## Annotation

### `@JsonProperty`

필드 또는 메서드에 사용되며, JSON 데이터와 매핑되는 속성의 이름을 지정합니다.

### `@JsonInclude`

직렬화 시 포함할 필드를 지정

### `@JsonFormat`&#x20;

어노테이션을 사용하여 날짜/시간 형식을 지정할 수 있습니다.

### `@JsonIgnore`

필드 또는 메서드에 사용되며, JSON 직렬화 및 역직렬화에서 해당 속성을 무시합니다.

### `@JsonAutoDetect`&#x20;

필드나 메서드의 접근 제한자에 따라 직렬화/역직렬화할 대상을 자동으로 감지합니다.

### `@JsonAlias`

JSON 데이터에서 여러 개의 속성 이름을 동일한 필드에 매핑할 수 있도록 지정합니다.

### `@JsonIgnoreProperties`

JSON 직렬화/역직렬화 시 무시할 속성들을 지정합니다.

### `@JsonRawValue`

JSON 데이터를 직접 문자열로 설정합니다.

### `@JsonManagedReference`와 `@JsonBackReference`

양방향 관계의 객체에서 순환 참조를 해결합니다.

### `@JsonGetter`

메서드에 사용되며, 해당 메서드가 직렬화 시에 속성의 값을 가져오는 메서드임을 나타냅니다.

### `@JsonSetter`

메서드에 사용되며, 해당 메서드가 역직렬화 시에 속성의 값을 설정하는 메서드임을 나타냅니다.

### `@JsonAnyGetter`

Map 형태의 필드에 사용되며, 해당 필드의 모든 속성을 JSON 객체로 직렬화할 때 사용됩니다.

### `@JsonAnySetter`

Map 형태의 메서드에 사용되며, 역직렬화 시에 속성을 Map에 동적으로 추가할 때 사용됩니다.

### `@JsonTypeId`

역직렬화 시에 객체의 타입을 지정하는 데 사용됩니다.

### `@JsonTypeName`

객체의 타입에 해당하는 값을 지정하는 데 사용됩니다.

## Reference

{% embed url="https://github.com/next-step/atdd-subway-favorite/pull/183#discussion_r805193770" %}
