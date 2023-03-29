# Annotation

Annotation이란 주석이란 뜻으로, 인터페이스를 기반으로 한 문법이다. 주석과는 그 역할이 다르지만 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다. 또 해석되는 시점을 정할 수도 있다.(Rentention Policy) Annotation에는 크게 세 가지 종류가 존재한다. JDK 에 내장되어 있는 built-in annotation과 annotation에 대한 정보를 나타내기 위한 annotation인 Meta-annotation 그리고 개발자가 직접 만들어 내는 Custom Annotation이 있다.



Annotations은 metadata를 구문과 연결하는 클래스 , 메서드 또는 필드와 같은 Java 구문에 적용되는 데코레이터입니다. 이러한 데코레이터는 양성이며 자체적으로 어떤 코드도 실행하지 않지만 런타임 프레임워크 또는 컴파일러에서 특정 작업을 수행하는 데 사용할 수 있습니다. 좀 더 공식적으로 말하면 Java 언어 사양(JlS), 섹션 9.7에서 다음관 같은 정의를 제공합니다.

> An anotation is a marker which associates information with a program construct, but has no effect at run time



## Built-in Annotation

* @Override
  * 메서드가 상속된 메서드의 동작을 재정의하거나 대체함을 나타내는 데 사용됩니다.
* @FunctionalInterface&#x20;
  * 인터페이스에 하나의 메서드를 나타내는 데 사용됩니다.
* @SuppressWarnings
  * 코드의 일부에서 특정 경고를 무시하고 싶다는 것을 나타냅니다.
* @SafeVarargs&#x20;
  * varargs 사용과 관련된 경고 유형에 적용됩니다.
* @Deprecated&#x20;
  * API를 더 이상 사용하지 않도록 표시할 수 있습니다.



## Meta-Annotations

* @Target&#x20;
  * 적용할 수 있는 대상을 설명합니다.
* @Retention&#x20;
  * 컴파일러에 의해 유지되어야 하는 기간을 설명합니다.
  * RetentionPolicy.SOURCE – 컴파일러와 런타임 모두에서 볼 수 없습니다.
  * RetentionPolicy.CLASS – 컴파일러에서 볼 수 있음&#x20;
  * RetentionPolicy.RUNTIME – 컴파일러와 런타임에서 볼 수 있습니다.
* @Inherited&#x20;
  * 상위 유형에 적용되는 경우 하위 유형에 의해 상속되어야 함을 나타냅니다.
* @Documented&#x20;
  * 기본적으로 Java는 Javadocs에서 @Annotation 사용을 문서화하지 않습니다.
  * 그러나 @Documented 주석을 사용하여 Java의 기본 동작을 변경할 수 있습니다.
* @Repeatable&#x20;
  * 동일한 컨텍스트에서 여러 번 적용할 수 있을 나타냅니다. 즉 클래스는 동일한 주석을 두 번 이상 적용할 수 있습니다.

## Reference

> [https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#annotation](https://github.com/JaeYeopHan/Interview\_Question\_for\_Beginner/tree/master/Java#annotation)
>
> [https://dzone.com/articles/5-annotations-every-java-developer-should-know](https://dzone.com/articles/5-annotations-every-java-developer-should-know)
>
> [https://dzone.com/articles/what-are-meta-annotations-in-java](https://dzone.com/articles/what-are-meta-annotations-in-java)
>
> [https://www.baeldung.com/java-default-annotations](https://www.baeldung.com/java-default-annotations)
