# @Autowired vs Constructor Dependency Injection

* 의존성이 명확하게 식별됩니다. 다른 환경에서 객체를 테스트하거나 인스턴스화할 때(구성 클래스에서 명시적으로 빈 인스턴스를 생성하는 것과 같은) 하나를 잊을 방법이 없습니다.
* 의존성은 최종적일 수 있으므로 견고성과 스레드 안전성에 도움이 됩니다.
* 의존성을 설정하기 위해 리플렉션이 필요하지 않습니다. InjectMocks는 여전히 사용 가능하지만 필수는 아닙니다. 직접 모의 객체를 생성하고 단순히 생성자를 호출하여 주입할 수 있습니다.

{% embed url="https://stackoverflow.com/questions/40620000/spring-autowire-on-properties-vs-constructor" %}
