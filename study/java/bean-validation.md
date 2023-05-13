---
description: 표준 JSR-380 프레임워크 및 Jakarta Bean Validation 3.0
---

# Bean Validation

## JSR 380

* JSR 380은 Jakarta EE 및 JavaSE의 일부인 빈 유효성 검사를 위한 Java API의 사양입니다. 이렇게 하면 @NotNull, @Min 및 @Max와 같은 annotations을 사용하여 빈의 속성이 특정 기준을 충족하는지 확인할 수 있습니다.
* 이 버전은 Hibernate-Validator 8.0.0을 제공하는 Spring Boot 3.x가 사용되므로 Java 17 이상이 필요합니다.
* 또한 stream, Optional Improvements ,modules, private interface methods 등과 같이 Java 9 이상에 도입된 많은 새로운 기능을 지원합니다.

## Dependencies

* spring-boot-starter-validation (latest version)
* hibernate-validator 8.0.0 Final

## Standard JSR annotations

* `@NotNUll` &#x20;
  * Annotation이 달린 속성 값이 null 아닌지 확인합니다.
* `@AssertTrue`&#x20;
  * Annotation이 달린 속성 값이 참인지 확인합니다.
* `@Size`&#x20;
  * Annotation이 달린 속성 값의 크기가 min속성과 max속성 상이에 있는지  확인합니다. String,Collection,Map 및  배열 속성에 적용할 수 있습니다.
* `@Min`&#x20;
  * Annotation 속성의 value 속성보다 작지 않는 값이 있는지 확인합니다.
* `@Max`&#x20;
  * Annotation 속성의 value 속성보다 크지 않은 값이 있는지 확인합니다.
* `@Email`
  * Annotation 속성이 유효한 이메일 주소인지 확인합니다.

## Additional annotations

* `@NotEmpty`&#x20;
  * 속성이 null이거사 비어 있지 않은지 확인합니다. String, Collection, Map 또는 Array 값에 적용할 수 있습니다.
* `@NotBlank`&#x20;
  * 텍스트 값에만 적용할 수 있으며 속성이 null 또는 whitespace이 아닌지 확인합니다.
* `@Positive`  and `@PositiveOrZero`&#x20;
  * 숫자 값에 적용하고 값이 양수인지 또는 0을 포함하여 양수인지 확인합니다.
* `@Negative`  and `@NegativeOrZero`&#x20;
  * 숫자 값에 적용되며 값이 완전히 음수인지 또는 0을 포함하여 음수인지 확인합니다.
* `@Past`  및 `@PastOrPresent`&#x20;
  * 날짜 값이 과거 또는 현재를 포함한 과거인지 확인합니다. Java 8에 추가된 날짜 유형을 포함하여 날짜 유형에 적용할 수 있습니다.
* `@Future` 및 `@FutureOrPresent`&#x20;
  * 날짜 값이 미래인지 또는 현재를 포함한 미래인지 확인합니다.

## Creating a Composed Constraint <a href="#bd-creating-a-composed-constraint" id="bd-creating-a-composed-constraint"></a>

```java
@NotNull
@Pattern(regexp = ".*\\d.*", message = "must contain at least one numeric character")
@Length(min = 6, max = 32, message = "must have between 6 and 32 characters")
@Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER })
@Retention(RUNTIME)
@Documented
@Constraint(validatedBy = {})
public @interface ValidAlphanumeric {

    String message() default "field should have a valid length and contain numeric character(s).";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
```

### _`@ReportAsSingleViolation`_ <a href="#bd-using-reportassingleviolation" id="bd-using-reportassingleviolation"></a>

* 단일 ConstraintViolation을 반환하도록 유효성 검사를 진행한다.

### `@ConstraintComposition(CompositionType.OR)`

* 적어도 하나의 제약조건을 만족 하는지 확인한다.

### _`@SupportedValidationTarget(ValidationTarget.ANNOTATED_ELEMENT)`_&#x20;

* 메서드의 반환 값을 확인한다.







## Reference

{% embed url="https://www.baeldung.com/javax-validation" %}

{% embed url="https://www.baeldung.com/java-bean-validation-constraint-composition#creating-a-composed-constraint" %}
