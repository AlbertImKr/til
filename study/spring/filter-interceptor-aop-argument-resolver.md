# Filter,Interceptor,AOP,Argument Resolver

## Filter:&#x20;

Servlet 기반의 웹 애플리케이션에서 사용되는 기능으로, HTTP 요청과 응답을 가로채서 요청과 응답을 가공하거나 필터링하는 역할을 합니다.&#x20;

* 예를 들어,&#x20;
  * 로깅, 인증, 인코딩 처리 등에 사용됩니다.
  * 요청에 대한 인코딩 처리를 하기 위해 CharacterEncodingFilter를 사용할 수 있습니다. 또는 Spring Security에서는 Filter를 사용하여 인증과 권한 체크를 수행합니다.
  * Request/Response의 헤더 정보를 변경하거나 캐시 기능을 구현하기 위해 Filter를 사용할 수 있습니다.

```java
@Component
public class CustomFilter implements Filter {
 
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // 필터 초기화 작업
    }
 
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        // 필터링 작업
        chain.doFilter(request, response);
    }
 
    @Override
    public void destroy() {
        // 필터 종료 작업
    }
}

```

## Interceptor:&#x20;

Spring MVC에서 컨트롤러의 호출 전후에 실행되는 기능으로, 요청 처리 전에 공통으로 수행해야 하는 작업을 구현할 때 사용됩니다.&#x20;

* 예를 들어,&#x20;
  * 로깅, 권한 체크, 트랜잭션 처리 등에 사용됩니다.
  * 로깅 기능을 구현하거나, 권한 체크를 수행하기 위해 Interceptor를 사용할 수 있습니다

```java
@Component
public class CustomInterceptor implements HandlerInterceptor {
 
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 컨트롤러 실행 전 작업
        return true;
    }
 
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // 컨트롤러 실행 후 작업
    }
 
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // 뷰 렌더링 후 작업
    }
}

```

## AOP:&#x20;

Aspect Oriented Programming의 약자로, 여러 모듈에서 공통으로 사용되는 기능을 모듈화하여 코드 중복을 제거하고 코드의 재사용성을 높이는 기술입니다. 메소드 실행 전후나 예외 발생 시점에 특정 코드를 삽입할 수 있습니다.&#x20;

* 예를 들어,&#x20;
  * 로깅, 트랜잭션 처리, 보안 등에 사용됩니다.
  * HTTP 요청에서 전달되는 인자 값을 변환하거나, 컨트롤러 메소드의 파라미터에 복잡한 객체를 매핑하기 위해 Argument Resolver를 사용할 수 있습니다.

```java
@Aspect
@Component
public class CustomAspect {
 
    @Around("execution(* com.example.service.*.*(..))")
    public Object doAround(ProceedingJoinPoint joinPoint) throws Throwable {
        // 메소드 실행 전 처리 작업
        Object result = joinPoint.proceed();
        // 메소드 실행 후 처리 작업
        return result;
    }
}

```

## Argument Resolver:&#x20;

Spring MVC에서 컨트롤러의 메소드 파라미터와 HTTP 요청(request)의 파라미터를 연결하여 파라미터 값을 전달하는 기능입니다.&#x20;

* 예를 들어,&#x20;
  * @RequestParam, @ModelAttribute, @PathVariable 등의 애노테이션과 함께 사용됩니다.
  * @RequestParam을 사용하여 요청 파라미터를 받아와 처리한다

```java
@Component
public class CustomArgumentResolver implements HandlerMethodArgumentResolver {
 
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        // 변환할 수 있는 타입인지 확인
        return parameter.getParameterType().equals(CustomArgument.class);
    }
 
    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        // 변환 작업
        CustomArgument customArgument = new CustomArgument();
        customArgument.setParam1(webRequest.getParameter("param1"));
        customArgument.setParam2(webRequest.getParameter("param2"));
        return customArgument;
    }
}

```

## Filter, Interceptor, AOP의 장단점

Filter:

* 장점:
  * 요청 처리 전/후에 일괄적인 로직을 적용할 수 있어서 보안, 인증, 로깅 등의 처리를 구현하기 용이함
  * 코드 중복을 방지할 수 있음
* 단점:
  * Spring ApplicationContext에 접근할 수 없음
    * Spring ApplicationContext에 직접적으로 접근할 수 없기 때문에, 일부 기능들을 사용할 수 없다. Spring ApplicationContext는 스프링에서 중요한 역할을 수행합니다. ApplicationContext를 이용하여 Bean을 생성하고, 관리할 수 있습니다. Filter는 서블릿 컨테이너에서 실행되기 때문에 ApplicationContext에 직접적으로 접근할 수 없으며, 따라서 ApplicationContext를 이용한 기능들을 사용할 수 없는 제약이 있습니다.
  * DispatcherServlet 이전에 동작하기 때문에 Spring MVC의 다른 기능을 사용할 수 없음
    * Filter는 DispatcherServlet 이전에 실행됩니다. 이로 인해 DispatcherServlet 이후에 실행되는 Spring MVC의 다른 기능들을 사용할 수 없는 등의 제약이 있습니다. 예를 들어, HandlerInterceptor는 DispatcherServlet 이후에 실행되기 때문에, Filter에서 처리할 수 없는 일부 기능들을 처리할 수 있습니다.

Interceptor:

* 장점:
  * HandlerMapping, HandlerAdapter 등 Spring MVC의 다른 기능과 결합하여 사용 가능
  * 요청 처리 전/후에 일괄적인 로직을 적용할 수 있어서 보안, 인증, 로깅 등의 처리를 구현하기 용이함
* 단점:
  * Spring ApplicationContext에 직접적으로 접근할 수 없음
    * Interceptor는 Filter와 마찬가지로 서블릿 컨테이너에서 실행되기 때문에, ApplicationContext에 직접적으로 접근할 수 없습니다. ApplicationContext를 이용한 기능들을 사용할 수 없는 제약이 있습니다.

AOP:

* 장점:
  * 공통적인 기능을 별도의 모듈로 분리하여 재사용성을 높일 수 있음
  * 비즈니스 로직 외의 기능(트랜잭션 처리, 로깅, 보안, 캐싱 등)을 구현하는 데 사용하기 용이함
  * OOP의 다형성과 같이 유연한 프로그래밍을 할 수 있어서, 객체지향적 설계에 기여할 수 있음
  * 기존 코드를 수정하지 않고도 새로운 기능을 추가할 수 있음
* 단점:
  * Proxy를 이용해 구현하기 때문에 메소드 호출 속도가 느려질 수 있음
    * 이는 대규모 애플리케이션에서 성능에 영향을 줄 수 있습니다. AOP는 대부분 Proxy를 이용하여 구현합니다. Proxy를 이용하면 호출하는 메소드를 래핑하여 추가적인 작업을 수행할 수 있습니다. 하지만 이로 인해 메소드 호출 속도가 느려질 수 있으며, 이는 대규모 애플리케이션에서 성능에 영향을 줄 수 있습니다.
  * 동적 프록시 기반의 구현 방법을 사용하기 때문에, 일부 기능에서는 제한적임
    * 모든 기능에 대해 적용하기에는 제한적일 수 있습니다.
  * AOP 자체가 복잡한 개념이기 때문에, 처음에는 이해하기 어려울 수 있습니다. AOP는 어플리케이션의 여러 부분에서 반복되는 코드를 제거하고, 관심사를 분리하여 모듈화하는 기법입니다. 이를 통해 코드의 유지보수성을 향상시키고, 코드의 가독성을 높일 수 있습니다.
  * 로그와 같은 디버깅 정보가 정확하지 않을 수 있어서 디버깅이 어려울 수 있음
    * 또한 AOP는 디버깅이 어렵고, 성능 저하가 발생할 수 있는 등의 단점이 있습니다. 따라서 AOP를 사용할 때는 신중하게 판단하여 적용해야 합니다.
