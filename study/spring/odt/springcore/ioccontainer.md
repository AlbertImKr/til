---
description: Version 6.0.7
---

# IoCContainer

## 1. Spring Ioc 컨테이너 및 Bean 소개



이 장에서는 Inversion of Controller(IoC) 원칙의 Spring Framework 구현을 다룹니다. IoC는 의존성 주입(DI)이라고 합니다. 객체가 생성자 인수, 팩토리 메서드에 대한 인수 또는 생성된 객체 인스턴스나 팩토리 메서드에서 반환된 객체 인스턴스에 설정된 속성을 통해서만 객체의 의존성(즉, 그들과 함께 일하는 다른 객체)을 정의하는 포로세스입니다. 그런 다음 컨테이너는 빈을 생성할 때 이러한 이존성을 주입합니다. 이 프로세스는 근본적으로 서비스 로케이터 패턴과 같은 메커니즘 또는 클래스의 직접 구성을 통하여 의존성의 인스턴스화 또는 위치를 제어하는 빈 자체의 역전(따라서 이름, Inversion of Control)입니다.



`org.springframeworkd.beans` 및 `org.springframework.context` 패키지는 Spring Framework의 IoC 컨테이너의 기반입니다. [BeanFactory](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/beans/factory/BeanFactory.html) 인터페이스는 모든 유형의 객체를 관리할 수 있는 고급 구성 메커니즘을 제공합니다. [ApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/ApplicationContext.html)는 `BeanFactory`의 하위 인터페이스입니다.

ApplicationContext 다음을 추가한다.

* Spring의 AOP 기능과 더 쉽게 통합
* 메시지 리소스 처리(국제화에 사용)
* 이벤트 발행
* 애플리케이션 계층 특정 Context 예를 들어 웹 애플리케이션에서 사용하는 `webApplicationContext`

즉, `BeanFactory`는 구성 프레임워크와 기본 기능을 제공하고 `ApplicationContext` 더 많은 enterprise-specific 기능을 추가합니다. `ApplicationContext`는 `BeanFactory`의 완성된 상위집합이며 이 장에서는 오직 Spring의 IoC컨테이너를 설명하기 위해 사용됩니다. `BeanFactory` 대신 `ApplicationContext` 사용에 대한 자세한 내용은 [BeanFactory API](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-beanfactory) 섹션을 참조하십시오.



Spring에서 애플리케이션의 중추를 형성하고 Spring IoC 컨테이너에 의해 관리되는 객체를 빈이라고 합니다. Bean은 Spring IoC 컨테이너에 의해 인스턴스화, 조립 및 관리되는 객체입니다. 그렇지 않으면 빈은 애플리케이션의 많은 객체 중 하나일 뿐입니다. Bean과 이들 간의 의존성은 컨테이너에서 사용하는 Configuration Metadata에 반영됩니다.



## 2. Container OverView



`org.springframework.context.ApplicationContext` 인터페이스는 Spring IoC 컨테이너를 표현하며 beans 인스턴스화, 구성 및 조립을 담당합니다. 컨테이너는 Configuration Metadata를 읽어 객체를 인스턴스화, 구성 및 어셈블리한다. Configuration Metadata는 XML , Java annotations 또는 Java코드로 표시한다. application을 구성하는 객체와 객체 간의 풍부한 상호 의존성을 표현할 수 있습니다.\


`ApplicationContext` 인터페이스의 여러 구현은 Spring과 함께 제공됩니다. 독립 실행형 Applications에서는 일반적으로 [ClassPathXmlApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html) 또는 [FileSystemXmlApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html)의 인스턴스를 만든다. XML은 Configuration Metadata를 정의하는 전통적인 형식이지만 소량의 XML 구성을 통하여 컨테이너에 Java annotations 또는 코드 같은 추가된 Metadata 형식을 사용할 수 있도록 할 수 있다.



대부분의 애플리케이션 시나리오에서 하나 이상의 Spring IoC 컨테이너 인스턴스를 인스턴스화하는 데 명시적인 사용자 코드가 필요하지 않습니다. 예를 들어, 웹 애플리케이션 시나리오에서 일반적으로 애플리케이션의 `web.xml` 파일에 있는 간단한 8줄 정도의 상용구 웹 설명자의 XML로도 충분합니다([Convenient ApplicationContext Instantiation for Web Applications](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-create) 참조). [Spring Tools for Eclipse](https://spring.io/tools)(Eclispse 기반 개발 환경)을 사용하면 몇 번의 마우스 클릭이나 키 입력으로 이 상용구 구성을 쉽게 만들 수 있습니다.



다음 다이어그램은 Spring이 작동하는 방식에 대한 높은 수준의 뷰를 보여줍니다. 애플리케이션 클래스는 Configuration Metadata와 결합되어 `ApplicationContext`를 생성하고 초기화된 후 완전히 구성되고 실행 가능한 시스템 또는 애플리케이션을 만든다.

<figure><img src="https://blog.kakaocdn.net/dn/cQmWQz/btr5R1y8QQ7/QKNik0K3USF3e95uzrVqNk/img.png" alt=""><figcaption></figcaption></figure>

## **2.1 Configuration Metadata**



앞의 다이어그램에서 볼 수 있듯이 Spring IoC 컨테이너는 Configuration metadata 형식을 사용합니다. 이 configuration metadata는 애플리케이션 개발자로서 애플리케이션에서 객체를 인스턴스화, 구성 및 어셈블 하도록 Spring 컨테이너에 지시하는 방법을 표시합니다.



Configuration metadata는 전통적으로 간단하고 직관적인 XML 형식으로 제공되며 이 장의 대부분은 Spring IoC 컨테이너의 주요 개념과 기능을 전달하기 위해 사용됩니다.



{% hint style="info" %}
XML 기반 metadata는 유일하게 허용되는 configuration metadata 형식이 아닙니다. Spring IoC 컨테이너 자체는 이 configuration metadata가 실제로 작성한 형식에서 완전히 분리됩니다. 요즘 많은 개발자들이 Spring 애플리케이션을 위해 [Java-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java)을 선택합니다.
{% endhint %}



Spring 컨테이너에서 다른 형식의 metadata를 사용하는 방법에 대한 자세한 내용은 다을 참조하세요.

* [Annotation-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config): annotation-based configuration metadata를 사용하여 beans을 정의합니다.
* [Java-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java): XML 파일이 아닌 Java를 사용하여 애플리케이션 클래스 외부에서 beans를 정의합니다. 이러한 기능을 사용하려면 [@Configuration](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Configuration.html), [@Bean](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Bean.html), [@Import](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Import.html), and [@DependsOn annotations](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/DependsOn.html)을 참조하십시오.



Spring configuraion은 컨테이너가 관리해야 하는 적어도 하나 일반적으로 둘 이상의 bean definition로 구성됩니다. XML-based configuration metadata는 이러한 빈을 최상위 요소 `<beans/>`내의 `<bean/>`요소로 구성됩니다. Java configuration은 일반적으로 `@Configuration` 클래스 내에서 `@Bean` -annotated 메서드를 사용합니다.



이러한 bean definitions는 애플리케이션을 구성하는 실제 객체에 해당합니다. 일반적으로 서비스 계층 객체, 저장소 또는 DAO(데이터 액세스 개체)와 같은 지속성 계층 객체, 웹 컨트롤러와 같은 프레젠테이션 객체, JPA `EntityManagerFactory`와 같은 인프라 객체, JMS 대기열 등을 정의합니다. 일반적으로 도메인 객체를 만들고 로드하는 것은 repositories와 비즈니스 로직의 책임이기 때문에 컨테이너에 세부적인 도메인 객체를 구성하지 않습니다.



다음 예는 XML-based configuration metadata의 기본 구조를 보여줍니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  ❶ ❷
        <!-- 이 bean에 대한 협력자 및 configuration은 여기로 넣습니다. -->
    </bean>

    <bean id="..." class="...">
        <!-- 이 bean에 대한 협력자 및 configuration은 여기로 넣습니다. -->
    </bean>

    <!-- 더 많은 bean definitions은 여기로 넣습니다.-->

</beans>
```

❶ id 속성은 개별 bean definition을 식별하는 문자열입니다.

❷ class 속성은 Bean의 타입을 정의하고 완전한 클래스 이름을 사용합니다.



id 속성의 값은 협업 객체를 참조하는 데 사용할 수 있습니다. 공동 작업 객체를 참조하기 위한 XML은 이 예제에 표시되지 않습니다. 자세한 내용은 [Dependencies](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-dependencies)을 참조하세요.



## **2.2 컨테이너 인스턴스화**



`ApplicationContext` 생성자에 제공되는 위치 경로는 컨테이너가 로컬 파일 시스템, Java `CLASSPATH` 등과 같은 다양한 외부 리소스에서 configuration metadata를 로드할 수 있도록 하는 리소스 문자열입니다.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

{% hint style="info" %}
Spring의 IoC 컨테이너에 대해 배운 후 URI 구문에 정의된 위치에서 InputStream을 읽기 위한 편리한 메커니즘을 제공하는 Spring의 Resource 추상화([Resources](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#resources) 참조)에 대해 더 알고 싶을 수 있습니다. 특히 Resource 경로는 애플리케이 contexts(\
[Application Contexts and Resource paths](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#resources-app-ctx) 참조)을 구성하는 데 사용됩니다.
{% endhint %}



다음 예는 서비스 계층 객체(`services.xml`) configuration 파일을 보여줍니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 서비스 -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- 이 bean에 대한 협력자 및 configuration은 여기로 넣습니다. -->
    </bean>

    <!-- 더 많은 bean definitions은 여기로 넣습니다. -->

</beans>
```

다음 예는 데이터 액세스 객체 `daos.xml` 파일을 보여줍니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- 이 bean에 대한 협력자 및 configuration은 여기로 넣습니다. -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- 이 bean에 대한 협력자 및 configuration은 여기로 넣습니다. -->
    </bean>

    <!-- 더 많은 bean definitions은 여기로 넣습니다. -->

</beans>
```

앞의 예에서 서비스 계층은 `PetStoreServiceImpl` 클래스와 `JpaAccountDao` 및 `JpaItemDao` 유형의 두 데이터 액세스 객체(JPA 객체-관계형 매핑 표준 기반)로 구성됩니다. `property name` 요소는 JavaBean 속성의 이름을 참조하고 `ref` 요소는 다른 bean정의의 이름을 참조합니다. `id`와 `ref` 요소 간의 이러한 연결은 공통 작업 개체 간의 의족성을 나타냅니다. 객체의 의존성 구성에 대한 자세한 내용은 [Dependencies을](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-dependencies) 참조하십시오.



### **XML-based Configuration Metadata 구성**



Bean definitions가 여러 XML 파일에 걸쳐 있는 것이 유용할 수 있습니다. 각 XML 구성 파일은 종종 아키텍처의 논리적 계층 또는 모듈을 표현합니다.



애플리케이션 context 생성자를 사용하여 이러한 모든 XML 조각에서 bean definitions을 로드할 수 있습니다. 이 생성자는 [이전 세션](ioccontainer.md#2.2)에 표시된 것처럼 여러 `Resource` 위치를 사용합니다. 또는 하나 이상의 `<import/>` 요소를 사용하여 다른 파일에서 bean definitions를 정의합니다. 다음 예에서는 이를 수행하는 방법을 보여줍니다.

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

예제에서 외부 bean definitions는 `services.xml`, `messageSource.xml` 및 `themeSource.xml`의 세 파일에서 로드됩니다. 모든 위치 경로는 importing을 수행하는 definition파일과 관계있다. 즉 `services.xml`은 importing을 수행하는 파일과 동일한 디렉터리 또는 클래스 위치에 있어야 하며 `messageSource.xml` 및 `themeSource.xml`은 importing 파일 위치 아래의 `resources` 위치 안에 있어야 한다. 보시다시피 선행 슬래시는 무시됩니다. 그러나 이러한 경로가 상대적인 경우 슬래시를 전형 사용하지 않는 것이 더 좋습니다. 최상위 레벨 `<beans/>` 요소를 포함하여 imported 한 파일의 내용은 Spring Schema에 따라 유효한 XML bean definitions여야 한다.



{% hint style="info" %}
상대 "../" 경로 사용하여 상위 디렉터리의 파일을 참조하는 것은 가능하지만 권장하지 않습니다. 이렇게 하면 현재 응용 프로그램 외부에 있는 파일에 대한 의존성이 생성됩니다. 특히 런타임 확인 프로세서를 통하여 "가장 가까운" 클래스 경로 root를 선택 후 다음 상위 디렉터리를 찾는 <mark style="background-color:yellow;">****</mark>`classpath`: URLs(예: `classpath:../services.xml`)의 참조를 권장하지 않습니다. Classpath configuration 변경으로 인한 다른 잘못된 디렉터리가 선택될 수 있습니다.\
\
상대 경로 대신 정규회 된 리소스 위치를 사용할 수 있습니다. 예: `file:C:/config/services.xml` or `classpath:/config/services.xml`. 그러나 애플리케이션의 configuration을 특정 절대 위치에 연결하고 있다는 점에 유의하십시오.  일반적으로 이러한 절대 위치에 대한 간접 참조를 유지하는 것이 좋습니다.  예를 들어 런타임 시 JVM 시스템 속성으로  확인하는  "${... }" placeholders를 사용한다.
{% endhint %}

namespace 자체는 import 지시 기능을 제공합니다.  일반 bean definitions을 넘어서는 추가 기능은 Spring에서 제공하는 XML namespaces(예: `context` 및 `util` namespaces)에서 사용할 수 있습니다.

### **The Groovy Bean Definition DSL**



외부 Configuration metadata에 대한 추가 예로는 bean difnitions는 Gralis 프레임워크에서 알려진 것처럼 Spring의 Groovy Bean Definition DSL로 표현할 수 있습니다. 일반적으로 이러한 구성은 다음 예제에 표시된 구조와 같은  ". groovy"파일이 있습니다.

```groovy
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

이 configuration 스타일은 대체로 XML bean definitions와 동일하며 Spring의 XML configuration namespaces도 지원합니다.또한 `importBeans` 명령을 통해 XML bean definition 파일을 imporing할 수 있습니다.



## **2.3 컨테이너 사용**



`ApplicationContext`는 서로 다른 beans과 그 의존성의 레지스트리를 잘 유지할 수 있는 고급 factory 인터페이스입니다. `T getBean(String name, Class <T> requiredType)` 메서드를 사용하여 beans의 인스턴스룰 검색할 수 있습니다.\


`ApplicationContext`를 사용하면 다음 예제와 같이 bean definitions를 읽고 액세스 할 수 있습니다.

```java
// beans 생성 및 구성
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// 구성된 인스턴스 검색
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// 구성된 인스턴스 사용
List<String> userList = service.getUsernameList();
```

Groovy configuraion 사용도 bootstrapping은 매우 유사해 보입니다. Groovy-aware(하지만 XML bean definitions도 이해한다.) 클래스를 구현하는 다른 context도 있습니다. 다음 예는 Groovy configuration를 보여줍니다.

```java
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

가장 유연한 변형은 reader delegates와 결합된 `GenericApplicationContext`입니다. 예를 들어 다음 예제와 같이 XML 파일용 `XmlBeanDefinitionReader`를 사용합니다.

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

다음 예제와 같이 Groovy 파일용 `GroovyBeanDefinitionReader`를 사용할 수도 있습니다.

```java
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

동일한 `ApplicationContext`에서 이러한 reader delegates를 믹스 및 매치하여 다양한 구성 소스에서 bean definitions을 읽어 올 수 있습니다.



그런 다음 `getBean`을 사용하여 beans의 인스턴스를 검색할 수 있습니다. `ApplicationContext` 인터페이스는 bean을 검색하기 위한 몇가지 다른 메서드가 있지만 이상적으로 애플리케이션 코드에서 이를 사용해서는 안 됩니다. 실제로 애플이케이션 코드에는 `getBean()` 메서드에 대한 호출이 전혀 없어야 하며 따라서 Spring API에 대한 의존성이 전혀 없어야 한다.  예를 들어 Spring의 웹 프레임워크의 통합은 컨트롤러 및 JSF-managed beans과 같은 다양한 웹 프레임워크 구성 요소에 대한 의존성 주입을 제공하여 metadata(예: autowiring annotation)를 통해 특정 bean에 대한 의존성을 선언할 수 있습니다.



## 3. Bean Overview



Spring IoC 컨테이너는 하나 이상의 빈을 관리합니다. 이러한 beans는 컨테이너에 제공하는 configuration metadata(예: XML `<bean/>` definitions의 형식)로 생성됩니다.



컨테이너 자체 내에서 이러한 beans definitions는 `BeanDefinition` objects로 표현되고 아래와 같은(기타 정보 중) metadata가 포함된다.

* A package-qualified class name: 일반적으로 정의되는 Bean의 실제 구현 클래스입니다.
* Bean이 컨테이너에서 어떻게 동작해야 하는지를 나타내는 Bean 동작 구성 요소 (scope, lifecycle callbacks 등)
* bean이 작업을 수행하는데 필요한 다른 beans에 대한 참조. 이러한 참조를 협력자 또는 의존성이라고 합니다.
* 새로 생성된 객체에 설정할 기타 configuration 설정 - 예: 풀의 크기 제한 또는 연결 풀을 관리하는 bean에서 사용할 connections의 개수

이 metadata는 각 bean definition을 구성하는 속성 집합으로 변환됩니다. 다음 표에서는 이러한 속성에 대해 설명합니다.



_Table 1. The bean definition_

| Property                  | 참조                                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Class                     | [Instantiating Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-class) |
| Name                      | Naming Beans                                                                                                             |
| Scope                     | Bean Scopes                                                                                                              |
| Constructor arguments     | Dependency Injection                                                                                                     |
| Properties                | Dependency Injection                                                                                                     |
| Autowiring mode           | Autowiring Collaborators                                                                                                 |
| Lazy initialiazation mode | Lazy-initialized Beans                                                                                                   |
| Initialization method     | Initialization Callbacks                                                                                                 |
| Destruction method        | Destruction Callbacks                                                                                                    |

\
특정 bean을 생성하는 방법에 대한 정보를 포함하는 bean definitions 외에도 `ApplicationContext` 구현은 (사용자에 의해) 컨테이너 외부에서 생성된 기존 객체의 등록을 허용합니다. 이는 `DefaultListableBeanFactory` 구현을 반환하는 `getBeanFactory()` 메서드를 통해 ApplicationContext의 `BeanFactory`에 accessing 하여 실행합니다. `DefaultListableBeanFactory`는 `registerSingleton(..)` 및 `registerBeanDefinition(..)` 메서드를 통해 이 등록을 지원합니다.&#x20;



{% hint style="info" %}
Bean metadata와 수동으로 제공된 싱글톤 인스턴스는 컨테이너가 autowiring 및 내부 검사 단계 과정에서 제대로 추론할 수 있도록 가능한 한 빨리 등록해야 합니다. 기존 metadata 및 기존 싱글톤 인스턴스를 overriding 하는 것은 지원되지만 런타임 시 새로운 beans 등록은 (동시에 라이브로 factory를 accsss) 공식적으로 지원되지 않으며 동시 access 예외 및 빈 컨테이너의 일관성 없는 상태를 도래될 수 있습니다.
{% endhint %}



## 3.1 Naming Beans



모든 빈에는 하나 이상의 식별자가 있습니다. 이러한 식별자는 bean을 호스팅 하는 컨테이너 내에서 고유해야 합니다. bean에는 일반적으로 식별자가 하나만 있습니다. 그러나  둘 이상이 필요한 경우 추가 항목은 별칭으로 간주될 수 있습니다.&#x20;



XML-based configuration medata에서 `id` 속성, `name` 속성 또는 둘 다 사용하여 bean 식별자를 지정합니다.  `id` 속성을 사용하면 정확히 하나의 `id`를 지정할 수 있습니다. 일반적으로 이러한 영숫자 ('myBean', 'someService' 등)이지만 특수 문자도 포함할 수 있습니다. bean에 대한 다른 별칭을 도입하려는 경우 `name` 속성에 쉼표(`,`), 세미콜론(`;`) 또는 공백으로 구분하여 지정할 수도 있습니다. id 속성은 `xsd:string` 타입으로 지정되지만 bean `id` 고유성은 XML 파서가 아니라 컨테이너에 의해 강제됩니다.



Bean의 `name`이나 `id`를 제공할 필요가 없습니다. `name`이나 `id` 를 명시적으로 제공하지 않으면 컨테이너는 해당 빈에 대해 고유한 이름을 생성합니다. 그러나 `ref` 요소 또는 Service Locator 스타일로 조회를 사용하여 해당 bean을 이름으로 참조하려면 이름을 반드시 제공해야 합니다. 이름을 제공하지 않아도 되는 원인은 [inner beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-inner-beans) 및 [autowiring collaborators](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire) 과 관련이 있습니다.



{% hint style="info" %}
Bean Naming Conventions

컨벤션은 beans의 이름을 지정할 때 인스턴스 필드 이름에 대한 표준 Java 컨벤션을 사용하는 것입니다. 즉 빈 이름은 소문자로 시작하고 그 후에 camel-cased를 적용한다. 이러한 이름의 예로는 **accountManager**, **accountService**, **userDao**, **loginController** 등이 있습니다.



Bean의 이름을 일관되게 지정하면 configuration을 더 쉽게 읽고 이해할 수 있습니다. 또한 Spring AOP를 사용하면 이름으로 관련된 beans set에 대한 어드바이스를 적용할 때 많은 도움이 된다.
{% endhint %}

{% hint style="info" %}
클래스 경로에서 구성 요소 스캔을 사용하면 Spring은 앞에서 설명한 규칙에 따라 이름이 지정되지 않은 구성요소에 대한 bean names를 생성합니다. 기본적으로 간단한 클래스 이름을 사용하여 초기 문자를 소문자로 바꿉니다. 그러나 두 개 이상의 문자가 있고 첫 번째 문자와 두 번째 문자가 모두 대문자인 (일반적인 아닌) 특별한 경우에는 원래 대소문자가 유지됩니다. 이들은 java.beans.Introspector.decapitalize(Spring이 사용하고 있는)에 의해 정의된 것과 동일한 규칙입니다.
{% endhint %}



### Bean Definition 외부에서 Bean 별칭 지정



bean definition 자체에서 id 속성에 의해 지정한 최대 하나의 name과 name 속성에 있는 임의의 수의 다른 이름의 조합을 사용하여 bean에 대해 하나 이상의 이름을 제공할 수 있습니다. 이러한 이름은 동일한 빈에 대한 동등한 별칭일 수 있으며 애플리케이션의 각 구성 요소가 해당 구성 요소 자체에 특정한 빈 이름을 사용하여 공통 의존성을 참조하도록 하는 것과 같은 일부 상황에 유용합니다.



하지만 bean이 실제로 정의된 모든 별칭을 지정은 항상 적절한 것은 아닙니다. 다른 곳에서 정의된 bean에 대한 별칭을 도입하는 것이 대로 바람직합니다. 일반적으로 자기만의 object definitions set을 가지고 있는 분할 서브시스템으로 구성되어 있는 대규모 시스템에서 발생합니다. XML-based configuration metadata에서 \<alias/> 요소를 사용하여 이를 수행할 수 있습니다. 다음 예에서는 이름 수행하는 방법을 보여줍니다.

```xml
<alias name="fromName" alias="toName"/>
```



이 경우 `forName`이라는 빈(동일한 컨테이너에 있음)은 alias definition를 사용한 후 `toName`으로 참조될 수도 있습니다.



예를 들어 하위 시스템 A의 configuration metadata는 `subsystemA-dataSource`라는 이름으로 DataSource를 참조할 수 있습니다. 서브시스템 B에 configuration metadata는 `subsystemB-dataSource`라는 이름으로 DataSource를 참조할 수 있습니다. 이 두 하위 시스템을 모두 사용하는 main 애플리케이션을 구성할 때 main 애플리케이션은 `myApp-dataSource`라는 이름으로 DataSource를 참조합니다.  세 개의 이름이 모두 동일한 개체를 참조하도록 하려면 다음 alias definitions를 configuration metadata에 추가할 수 있습니다.

```xml
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
```

이제 각 구성 요소와 기본 애플리케이션은 고유하고 다른 definition와 충돌하지 않도록 보장되는(효과적으로 namespace를 생성) 이름을 통해 dataSource를 참조할 수 있고 동일한 bean도 참조할 수 있습니다.



{% hint style="info" %}
**Java-configuraion**

Java Configuraion를 사용하는 경우 @Bean annotation을 사용하여 별칭을 제공할 수 있습니다. 자세한 내용은 [Using the @Bean Annotation](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java-bean-annotation)을 참조하십시오.
{% endhint %}



## 3.2 Beans 인스턴스화



bean definition는 기본적으로 하나 이상의 객체를 생성하기 위한 레시피입니다. 컨테이너는 요청 시 명명된 bean에 대한 레시피를 보고 해당 bean definition에 의해 캡슐화된 configuraion metadata를 사용하여 실제 객체를 생성(또는 획득)합니다.



XML-based configuration metadata를 사용하는 경우 `<bean/>`요소의 `class` 속성에 인스턴스화할 객체의 타입(또는 클래스)을 지정합니다. 이 클래스 속성 (내부적으로 BeanDefinition 인스턴스의 Class 속성)은 일반적으로 필수입니다. (예외는 [Instantiation by Using an Instancee Factory Method](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-class-instance-factory-method)와 [Bean Definition Inheritance](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-child-bean-definitions) 참조하십시오). 두 가지 방법 중 하나로 Class 속성을 사용할 수 있습니다.

* 일반적으로 컨테이너 자체가 직접 반사적으로 생성자를 호출하여 bean을 직접 생성하는 경우에 생성될 bean 클래스를 지정하기 위해 `new` 연산자를 사용하는 Java 코드와 다소 동일합니다.
* 덜 일반적인 경우 객체를 생성하기 위해 호출되는 `static` 팩토리 메서드를 포함하는 실제 클래스를 지정하기 위해 컨테이너가 bean을 생성하기 위해 클래스에서 `static` 팩토리 메서드를 호출한다. `static` 팩토리 메서드 호출에서 반환된 객체 타입은 동일한 클래스이거나 완전히 다른 클래스일 수 있습니다.



{% hint style="info" %}
**중첩된 클래스 이름**

중첩된 클래스에 bean definition을 구성하려는 경우 중첩 클래스의 이진 이름 또는 소스 이름을 사용할 수 있습니다.



예를 들어 com.example 패키지에 `SomeThing`이라는 클래스가 있고 이 `SomeThing` 클래스에 `OtherThing`이라는 중첩 `static` 클래스가 있는 경우 달러 기호(`$`) 또는 점(`.`)으로 구분할 수 있습니다. 따라서 bean definition에서 클래스 속성의 값은 `com.example.SomeThing$OtherThing` 또는 `com.example.SomeThing.OtherThing`이 됩니다.
{% endhint %}



### 생성자를 사용한 인스턴스화



생성자 접근 방식으로 bean을 생성하면 모든 일반 클래스가 Spring에서 사용 가능하고 호환됩니다. 즉 개발 중인 클래스는 특정 인터페이스를 구현하거나 특정 방식으로 코딩할 필요가 없습니다. 단순히 bean 클래스를 지정하는 것으로 충분합니다. 그러나 특정 빈에 사용하는 IoC 유형에 따라 기본(empty) 생성자가 필요할 수 있습니다.



Spring IoC 컨테이너는 사실상 관리하기를 원하는 모든 클래스를 관리할 수 있습니다. 진정한 JavaBeans 관리에 국한되지 않습니다. 대부분의 Spring 사용자를 기본(인수 없는) 생성자와 컨테이너의 속성을 따라 적절한 setter 및 getter만 있는 진정한 JavaBeans를 선호합니다. 컨테이너에 더 이국적인 non-bean-style 클래스를 가질 수도 있습니다. 예를 들어 Javabean 명세서를 완전히 준수하지 않는 레거시 연결 풀을 사용해야 하는 경우 Spring도 이를 관리할 수 있습니다.



XML-based configuration metadata를 사용하여 다음과 같이 Bean 클래스를 지정할 수 있습니다.

```xml
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```



생성자에 인수를 제공하고(필요한 경우) 객체가 생성된 후 객체 인스턴스 속성을 설정하는 메커니즘에 대한 지세한 내용은 [Injecting Dependencies](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-collaborators)을 참조하십시오.



### 정적 팩토리 메서드를 사용한 인스턴스화



`static` 팩토리 메서드로 생성하는 bean을 정의할 때 클래스 속성을 사용하여 `static` 메소드를 포함하는 클래스를 지정하고 `factory-method`라는 속성을 사용하여 팩토리 메서드 자체의 이름을 지정합니다. (나중에 설명하는 선택적 인수를 사용하여) 이 메서드를 호출하고 활성 객체를 반환할 수 있어야 합니다. 이후에 이 객체는 생성자를 통해 생성된 것처럼 처리됩니다. 이러한 bean definition의 한 가지 용도는 레거시 코드에서 `static` 팩토리를 호출하는 것입니다.



다음 bean definition는 팩토리 메서드를 호출하여 빈이 생성되도록 지정합니다. definition은 반환된 객체의 타입(클래스)을 지정하지 않고 오히려 팩토리 메서드를 포함하는 클래스를 지정합니다. 이 예제에서 `createInstance()` 메서드는 `static` 메서드여야 한다. 다음 예제에서는 팩토리 메서드를 지정하는 방법을 보여줍니다.

```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```

다음 예제는 이전 bean definition와 함께 작동하는 클래스를 보여줍니다.

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```

팩토리 메서드에 인수(선택 사항)를 제공하고 객체가 팩토리에서 반환된 후 객체 인스턴스 속성을 설정하는 메커니즘에 대한 자세한 내용은 다음을 참조하십시오. [Dependencies and Configuration in Detail](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-properties-detailed).



### 인스턴스 팩토리 메서드를 사용한 인스턴스화

[정적 팩토리 메서드를 통한 인스턴스화](ioccontainer.md#undefined-1)와 유사하게 인스턴스 팩토리 메서드를 사용한 인스턴스화는 컨테이너에서 기존 빈의 비정적 메서드를 호출하여 새 bean을 생성합니다. 이 메커니즘을 사용하려면 `class` 속성을 비워두고 `factory-bean` 속성에서 객체를 생성하기 위해 호출할 인스턴스 메서드를 포함하는 현재(또는 부모 또는 조상) 컨테이너의 빈 이름을 지정합니다. `fatory-method` 속성으로 팩토리 메서드 자체의 이름을 설정합니다.&#x20;

다음 예제는 이러한 bean을 구성하는 방법을 보여줍니다.

```xml
<!-- createInstance()라는 메서드를 포함하는 팩토리 bean -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- 이 locator bean에 필요한 의존성을 주입합니다. -->
</bean>

<!-- 팩토리 빈을 통해 생성될 빈 -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

```

다음 예는 해당 클래스를 보여줍니다.

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```



다음 예제와 같이 하나의 팩토리 클래스는 둘 이상의 팩토리 메서드를 보유할 수도 있습니다.

```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- 이 locator bean에 필요한 의존성을 주입합니다. -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```

다음 예는 해당 클래스를 보여줍니다.

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```



이 접근 방식은 팩토리 빈 자체가 의존성 주입(DI)을 통해 관리되고 구성될 수 있음을 보여줍니다. [Dependencies and Configuration in Detail](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-properties-detailed)를 참조하십시오.



{% hint style="info" %}
Spring 문서에서 "factory bean"은 Spring 컨테이너에 구성되고 [instance](ioccontainer.md#undefined-2) 또는 [static](ioccontainer.md#undefined-1) 팩토리 메서드를 통해 객체를 생성하는 bean을 의미합니다.  대조적으로 FactoryBean(대소문자에 주의)은 Spring-specific [FactoryBean](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-extension-factorybean) 구현 클래스를 참조합니다.
{% endhint %}



### Bean의 런타임 타입을 결정

특정 빈의 런타임 타입은 결정하기 쉽지 않습니다. bean metadata definition에 지정된 클래스는 초기 클래스 참조일 뿐이며 선언된 팩토리 메서드와 결합되거나 또는 bean의 다른 런타임 타입으로 이어질 수 있는 `FactoryBean` 클래스이거나 인스턴스 레벨 팩토리 메서드(대신 지정된 factory-bean 이름을 통해 해결됩니다.)의 경우 전형 설정되지 않는다.  또한 AOP 프록싱은 대상 빈의 실제 유형(구현된 인터페이스만)을 제한적으로 노출하는 interface-based 프록시로 bean 인스턴스를 래핑 할 수 있습니다.



특정 bean의 실제 런타임 타입을 찾는 권장 방법은 지정된 bean 이름에 대한 `BeanFactory.getType` 호출입니다. 이것은 위의 모든 경우를 고려하고 BeanFactory.getBean 호출이 동일한 빈 이름에 대해 반환할 객체 타입을 반환합니다.



## 4. Dependencies



일반적으로 엔터프라이즈 애플리케이션은 단일 객체(또는 Spirng용어로 빈)로 구성되지 않습니다.  가장 단순한 애플리케이션이라도 최종 사용자에게 일관성 있는 애플리케이션을 보여주기 위하여 함께 작동하는 여러개의 개체가 있습니다.  다음 색션에서는 독립적인 여러 bean definitions을 defining하는 것부터 목표를 달성하기 위해 객체가 협력하는 완전히 구현된 애플리케이션으로 이동하는 방법을 설명합니다.



## 4.1 Dependency Injection



Dependency injection (DI)는 생성자 인수, 팩토리 메서드에 대한 인수 또는 팩토리 메서드에서 생성되거나 반환된 객체 인스턴스에 설정된 속성을 통해서만 객체의 의존성(즉 객체와 협업하고 있는 다른 객체)을 정의하는 프로세스입니다. 그런 다음 컨테이너는 빈을 생성할 때 이러한 의존성을 주입합니다. 이 프로세스는 기본적으로 클래스의 적집 구성 또는 서비스 로케이터 패턴을 사용하여 bean자체의 의존성 위치 또는 인스턴스화를 제어하는 bean 자체의 역전(따라서 이름, Inversion of Control)입니다. DI 원칙을 사용하면 코드가 더 깔끔해지며 객체에 의존성을 제공될 때 더 효과적으로 분리합니다. 객체는 의존성을 조회하지 않으면 의존성의 위치나 클래스를  알지 못합니다.  그 결과, 특히 의존성이 인터페이스 또는 추상 기본 클래스에 있을 때 클래스를 테스트하기가 더 숴워지며, 이를 통해 stub 또는 mock 구현을 단위테스트에 사용할 수 있습니다.



DI는 두 가지 주요 변형으로 존재합니다. [Constructor-based dependency injection](ioccontainer.md#constructor-based-dependency-injection)과 [Setter-based dependency injection](ioccontainer.md#setter-based-dependency-injection) 이 있습니다.



### **Constructor-based Dependency Injection**

생성자 기반 DI은 각각 의존성을 나타내는 여러 인수를 사용하여 생성자를 호출하는 컨테이너에 의해 달성됩니다.  Bean을 구성하기 위해 특정 인수로 `static` 팩토리 메서드 호출하는 것은 거의 동일하며 이 토론에서는 생성자와 `static` 팩토리 메서드에 대한 인수를 유사하게 취급합니다. 다음 예제는 생성자 주입을 통해서만 의존성 주입이 가능한 클래스를 보여줍니다.

```java
public class SimpleMovieLister {

    // SimpleMovieLister에는 MovieFinder에 대한 의존성이 있습니다.
    private final MovieFinder movieFinder;

    // Spring 컨테이너가 MovieFinder를 주입할 수 있도록 하는 생성자
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // 주입된 MovieFinder를 실제로 사용하는 비즈니스 로직은 생략
}
```

이 클래스는 특별한 것이 없습니다. 컨테이너 특정 인터페이스, 기본 클래스 또는 annotations에 대한 의존성이 없는 POJO입니다.



### Constructor Argument Resolution

생성자 인수 Resolution은 인수 유형을 사용하여 매칭합니다. Bean definition의 생성자 인수에 잠재적인 모호성이 없는 경우, 생성자 인수가 bean definition에서 정의되는 순서는 Bean이 인스턴스화될 때 해당 인수가 적절한 생성자에게 제공되는 순서입니다. 다음 클래스를 고려합니다.

```java
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```

ThingTwo 및 ThingThree 클래스가 상속에 의해 관련되지 않는다고 가정하면 잠재적 모호성이 존재하지 않습니다.  따라서 다음 구성은 제대로 작동하면 `<constructor-arg/>`요소에 새성자 인수 인덱스 또는 유형을 명시적으로 지정할 필요가 없습니다.

```xml
<beans>
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg ref="beanTwo"/>
        <constructor-arg ref="beanThree"/>
    </bean>

    <bean id="beanTwo" class="x.y.ThingTwo"/>

    <bean id="beanThree" class="x.y.ThingThree"/>
</beans>
```

다른 bean이 참조될 때 유형을 알고 있으면 매칭을 할 수 있습니다(이전 예제의 경우처럼). `<value>true</value>`와 같은 단순 유형을 사용하는 경우 Spring은 값의 유형을 판별할 수 업으므로 도움 없이 유형별로 매칭할수 없습니다. 다음 클래스를 고려합니다.

```java
package examples;

public class ExampleBean {

    // Ultimate Answer를 계산하는 데 걸리는 연수
    private final int years;

    // 생명,우주,모든것에 대한 해답
    private final String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```



#### 생성자 인수 유형 매칭



이전 시나리오에서 컨테이너는 다음 예제와 같이 `type` attribute을 사용하여 생성자 인수 type을 명시적으로 지정하는 경우 단순 타입과 타입 매칭을 사용할 수 있다.

```java
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```



#### 생성자 인수 인덱스



다음 예제와 같이 `index` attribute를 사용하여 생성자 인수의 인덱스를 명시적으로 지정할 수 있습니다.

```java
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

여러 단순 값의 모호성을 해결하는 것 외에도 index를 지정하면 생성자에 동일한 유형의 두 인수를 갖는 모호성도 해결할 수 있습니다.



{% hint style="info" %}
index는 0부터 시작합니다.
{% endhint %}

#### 생성자 인수 이름



다음 예제와 같이 값 명확성을 위해 생성자 매개 변수 이름을 사용할 수도 있습니다

```java
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

이 작업을 즉시 수행하려면 Spring이 생성자에서 매개변수 이름을 조회할 수 있도록 디버그 플래그를 사용하여 코드를 컴파일해야 합니다. 디버그 플래그로 코드를 컴파일할 수 없거나 컴파일하지 않으려면 [@ConstructorProperties ](https://docs.oracle.com/javase/8/docs/api/java/beans/ConstructorProperties.html)JDK annotation을 사용하여 생성자 인수의 이름을 명시적으로 지정할 수 있습니다. 그러면 샘플 클래스는 다음과 같아야 합니다.

```java
package examples;

public class ExampleBean {

    // 생략된 Fields

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}


```



### **Setter-based Dependency Injection**

setter기반 DI는 컨테이너가 빈을 인스턴스화하기 위해 인수 없는 생성자 또는 인수 없는 `static` 팩토리 메서드를 호출한 후 빈의 세터 메서드를 통하여 달성합니다. 다음 예제에서는 순수한 setter 주입을 사용해야만 의존성 주입할 수 있는 클래스를 보여 주입니다. 이 클래스는 일반적으로 Java입니다. 컨테이너 특정 인터페이스, 기본 클래스 또는 annotation에 대한 의존성이 없는 POJO입니다.

```java
public class SimpleMovieLister {

    // SimpleMovieLister에는 MovieFinder에 대한 의존성이 있습니다.r
    private MovieFinder movieFinder;

    // Spring 컨테이너가 MovieFinder를 주입할 수 있도록 하는 setter 메서드
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // 주입된 MovieFinder를 실제로 사용하는 비즈니스 로직은 생략...
}
```

ApplicationContext는 관리하는 Bean에 대해 생성자 기반 및 setter기반 DI를 지원합니다. 또한 생성자 접근 방식을 통해 일부 의존성이 이미 주입된 후 setter기반 DI를 지원합니다.  속성을 한 형식에서 다른 형식으로 변환하기 위해 PropertyEdito 인스턴스와 함께 사용하는 BeanDefinition 형식으로 의존성을 구성합니다.  그러나 대부분 Spring 사용자는 이러한 클래스를 직접(즉,프로그래밍 방삭으로) 작업하지 않고 XML bean definitions, annotatied 구성 요서(즉 @Component, @Controller 등으로 주석이 달린 클래스) 또는 Java기반 @Configuration 클래스의 @Bean 메서드를 사용합니다. 그런 다음 이러한 소스는 내부적으로 BeanDefinitiond의 인스턴스로 변환되고 전체 Spring IoC 컨테이너 인스턴스를 로드하는 데 사용됩니다.

{% hint style="info" %}
### 생성자 기반 or setter 기반 DI?

생성자 기반 DI와 setter 기반 DI를 혼합할 수 있으므로 필수 의존성에 대해서는 생성자를 사용하고 선택적  의존성에 대해서는 setter 메서드 또는 configuration 메서드를 사용하는것이 좋습니다. setter 메서드에서 @Autowired annotation을 사용하여 속성을 필수 의존성으로 만들 수 있습니다. 그러나 프로그래밍 방식으로 인수를 검증하는 생성자 주입이 더 좋습니다.



Spring 팀은 일반적으로 생성자 주입을 옹호하는데, 이는 애플리케이션 구성 요소를 불변 객체로 구현하고 필수 의존성이 null이 아님을 보장하기 때문이다. 또한 생성자 주입 구성 요소는 항상 완전히 초기화된 상태로 클라이언트(호출)코드에 반환됩니다. 참고로 많은 수의 생성자 인수는 나쁜 코드 냄새 입니다. 즉 클래스에 너무 많은 책임이 있을 수 있으며 문제의 적절한 분리를 더 잘 해결하기 위해 리팩토링해야 합니다.



Setter 주입은 기본적으로 클래스 내에서 합리적인 기본 값을 할당할 수 있는 선택적 의존성에만 사용해야 합니다. 그렇지 않으면 코드가 의존성을 사용하는 모든 곳에서 null이 아닌 검사를 수행해야 합니다. setter주입의 한 가지 이점은 setter 메서드가 해당 클래스 개체를 나중에 재구성하거나 다시 주입할 수 있도록 만든다는 것입니다. 따라서 [JMX MBeans](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#jmx)를 통해 관리는 setter 주입에 대한 강력한 사용 사례입니다.



특정 클래스에 가장 적합한 DI 스타일을 사용합니다. 때로는 소스가 없는 타사 클래스를 처리할 때 선택이 이루어집니다. 예를 들어 타사 클래스가 setter 메서드를 노출하지 않는 경우 생성자 주입이 유일하게 사용 가능한 DI 형식일 수 있습니다.
{% endhint %}

### **Dependency Resolution Process**

컨테이너는 다음과 같이 Bean 의존성 Resolution을 수행합니다.

* Application는 모든 bean을 설명하는 configuration metadata로 생성되고 초기화됩니다. Configuration metadata은 XML,Java 코드 또는 annotations로 지정할 수 있습니다.
* 각 빈에 대해 해당 의존성은 속성, 생성자 인수 또는 static-factory 메서드에 대한 인수의 형태로 표현됩니다.(일반 생성자 대신 사용하는 겨우). 이러한 의존성은 bean이 실제로 생성될 때 bean에 제공됩니다.
* 각 속성 또는 생성자 인수는 설정할 값의 실제 definition이거나 컨테이너의 다른 빈에 대한 참조입니다.&#x20;
* 값인 각 속성 또는 생성자 인수는 지정된 형식에서 해당 속성의 실제 타입으로 변환됩니다. 기본적으로 Spring은 문자열 형식으로 제공된 값을 int,long,String,boolean 등과 같은 모든 내장 유형으로 변환할 수 있습니다.

Spring 컨테이너는 컨테이너가 생성될 때 각 빈의 구성을 검증합니다. 그러나 bean 속성 자체는 bean이 실제로 생성될 때까지 설정되지 않습니다. 싱글톤 범위이고 pre-instantiated(기본값)로 설정된 Bean은 컨테이너가 생성될 때 생성됩니다.  범위는 [Bean Scopes](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes)에서 정의됩니다. 그렇지 않으면 요청될 때만 Bean이 생성됩니다. bean의 생성은 잠재적으로 bean의 의존성 및 해당 의존성의 의존성(등)이 생성되고 할당됨에 따라 bean의 그래프가 생성됩니다.  이러한 의존성 간의 해결 불일치는 늦게 나타날 수 있습니다. 즉, 영향을 받는 Bean을 처음 생성할 때 나타날 수 있습니다.

{% hint style="info" %}
### Circular dependencies

주로 생성자 주입을 사용하는 경우 행결할 수 없는 순환 의존성 시나리오를 만들 수 있습니다.



예를 들어 클래스 A에는 생성자 주입을 통해 클래스 B의 인스턴스가 필요하고 클래스 B에는 생성자 주입을 통해 클래스 A의 인스턴스가 필요합니다. 클래스 A와  B가 서로 주입되도록 빈을 구성하면 Spring IoC 컨테이너는 러타임에 이 순환을 참조를 감지하고 **BeanCurrentlyInCreationException**을 발생시킵니다.



한 가지 가능한 해결책은 생성자 아닌 setter가 구성할 일부 클래스의 소스 코드를 편집하는 것입니다. 또는 생성자 주입을 피하고 setter 주입만 사용합니다. 즉 권자하지는 않지만 setter 주입으로 순환 의존성을 구성할 수 있습니다.



일반적인 경우(순환 의존성이 없음)와 달리 bean A와 bean B 간의 순환 의존성은 bean 중 하나가 완전히 초기화되기 전에 다른 bean에 주입되도록 강제합니다.(전통적인 닭과 달걀의 시나리오)
{% endhint %}

일반적으로 Spring이 올바른 일을 한다고 믿을 수 있습니다. 컨테이너 로드 시 존재하지 않은 Bean 및 순환 의존성에 대한 참조와 같은 구성 문제를 감지하빈다. Spring은 Bean이 실제로 생성될 때 가능한 한 늦게 속성을 설정하고 의존성을 해결합니다. 즉 올바르게 로드된 Spring 컨테이너는 해당 객체 또는 해당 의존성 중 하나를 생성하는 데 문제가 있는 경우 객체를 요청할 때 나중에 예외를 생성할 수 있음을 의미합니다- 예를 들어 bean은 누락되거나 잘못된 속성의 결과로 예외를 던집니다. 일부 구성 문제에 대한 잠재적으로 지연된 가시성은 ApplicationContext 구현이 기본적으로 싱글톤 빈을 미리 인스턴스화하는 이유입니다. 실제로 필요하기 전에 이러한 빈을 생성하기 위한 약간의 선행 시간과 메모리 비용으로 나중에가 아니라 ApplicationContext가 생성될 때 구성 문제를 발견합니다. 이 기본 동작을 제정의하여 싱글톤 빈이 열심히 미리 인스턴스화되지 않고 느리게 초기화되도록 할 수 있습니다.



순환 의존성이 없는 경우 하나 이상의 협력 빈이 의존빈에 주입될 때 각 협력 빈은 의존 빈에 주입되기 전에 완전히 구성됩니다. 즉 bean A가 bean B에 대한 의존성을 가지고 있으면 Spring IoC 컨테이너는 bean A에서 setter 메서드를 호출하기 전에 bean B를 완전히 구성합니다. 즉 빈이 인스턴스화되고(미리 인스턴스화된 싱글톤이 아닌 경우) 해당 의존성이 설정되며 관련 수명 주기 메서드(예: [configured init method](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-initializingbean) 또는 [InitializaingBean callback method](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-initializingbean))가 호출됩니다.

### **Examples of Dependency Injection**

다음 예에서는 setter 기반 DI에 XML 기반 configuration metadata를 사용합니다. Spring XML 구성 파일의 작은 부분은 다음과 같이 일부 bean 정의를 지정합니다.

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- 중첩된 ref 요소를 사용한 setter 주입 -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- 더 깔금한 ref속서을 사용한 주입 -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

다음 예제는 해당하는 ExampleBean 클래스를 보여줍니다.

```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```



앞의 예에서 setter는 XML 파일에 지정된 속성과 일치하도록 선언됩니다. 다음 예에서는 생성자 기반 DI를 사용합니다.

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- 중첩된 ref 요소를 사용한 setter 주입 -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>

    <!-- 더 깔금한 ref속서을 사용한 주입 -->
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>

```

다음 예제는 해당하는 ExampleBean 클래스를 보여줍니다.

```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public ExampleBean(AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }
}
```

Bean definition에 지정된 생성자 인수는 ExampleBean의 생성자에 대한 인수로 사용됩니다.



이제 생성자를 사용하는 대신 객체의 인스턴스를 반환하기 위해 정적 팩토리 메서드를 호출하도록 Spring에 지시하는 이 예제의 변형을 고려합니다.

```xml
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

다음 예제는 해당하는 ExampleBean 클래스를 보여줍니다.

```java
public class ExampleBean {

    // private 생성자
    private ExampleBean(...) {
        ...
    }

    // 정적 팩토리 메소드; 이 메소드에 대한 인수는
    // 해당 인수가 실제로 사용되는 방법에 관계없이 반환되는
    // Bean의 의존성으로 간주될 수 있습니다.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // 다른 작업...
        return eb;
    }
}
```

`static` 팩터리 메서드에 대한 인수는 `<constructor-arg/>` 요소에 의해 제공되며 마치 생성자가 실제로 사용된 것과 정확히 동일합니다. 팩터리 메서드에서 반환되는 클래스의 타입은 `static` 팩터리 메서드를 포함하는 클래스와 같은 `type`일 필요는 없습니다(이 예제에서는 동일하지만). 인스턴스(non-static) 팩토리 메서드는 본질적으로 동일한 방식으로 사용될 수 있으므로(class 속성 대신 factory-bean 속성을 사용하는 것 제외) 여기서는 이러한 세부 사항을 노의하지 않습니다.



## 4.2 Dependencies and Configuration in Detail

[이전 섹션](ioccontainer.md#4.1-dependency-injection)에서 언급한 것 처럼 다른 빈(협력자)에 대한 참조 또는 인라인으로 정의된 값으로 빈 속성 및 생성자 인수를 정의할 수 있습니다. Spring의 XML 기반 configuration metadata는 이러한 목적을 위해 `<property/>`및 `<constructor-arg/>` 요소 내의 하위 요소 유형을 지원합니다.



### **Straight Values (Primitives, Strings, and so on)**

`<property/>` 요소의 값 특성은 사람이 읽을 수 있는 문자열 표현으로 속성 또는 생성자 인수를 지정합니다. Spring의  [conversion service](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#core-convert-ConversionService-API)는 이러한 값을 문자열에서 속성 또는 인수의 실제 유형으로 변환하는 데 사용됩니다. 다음 예에서는 설정되는 다양한 값을 보여줍니다.

```java
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- setDriverClassName(String) 호출 결과 -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="misterkaoli"/>
</bean>
```

다음 예제에서는 훨씬 더 간결한 XML 구성을 위해 p-namespace를 사용합니다.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="misterkaoli"/>

</beans>
```

앞의 XML이 더 간결합니다. 그러나 bean definitions를 생성할 때 자동 속성 완성을 지원하는 IDE(예: IntelliJ IDEA 또는 Spring Tools for Eclipse)를 사용하지 않는 한 디자인 타임이 아닌 런타임에 오타가 발견됩니다. 이러한 IDE 보조를 적극 권장합니다.



다음과 같이 **java.util.Properties** 인스턴스를 구성할 수도 있습니다.

```xml
<bean id="mappings"
    class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">

    <!-- java.util.Properties로 입력됨 -->
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```



Spring 컨테이너는 JavaBeans `PropertyEditor` 메커니즘을 사용하여 `<value/>` 요소 내부의 텍스트를 `java.util.Properties` 인스턴스로 변환합니다. 이것은 좋은 지름길이며 Spring 팀이 `value` 속성 스타일보다 중첩된 \<value/> 요소의 사용을 선호하는 몇 가지 중의 하나입니다.



### The iderf element

`idref` 요소는 컨테이너에 있는 다른 빈의 id(참조가 아닌 문자열 값)를 `<constructor-arg/>`또는 `<property/>` 요소로 전달하는 오류 방지 방법입니다. 다음 예제는 사용 방법을 보여줍니다.

```xml
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean"/>
    </property>
</bean>
```

앞의 빈 정의 스니펫은 다음 스니펫과 정확히 (런타임 시)동일합니다.

```xml
<bean id="theTargetBean" class="..." />

<bean id="client" class="...">
    <property name="targetName" value="theTargetBean"/>
</bean>
```

첫 번째 형식은 두 번째 형식보다 선호되는데, `idref` 태그를 사용하면 배포 시 컨테이너가 참조되고 명명된 bean이 실제로 존재하는지 확인할 수 있기 때문입니다. 두 번째 변형에서는 클라이언트 빈의 targetName 속성에 전달된 값에 대해 유효성 검사가 수행되지 않습니다. 오타는 클라이언트 빈이 실제로 인스턴스화될 때만 발견됩니다(치명적인 결과가 발생할 가능성이 가장 높음). 클라이언트 빈이 [prototype](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes) 빈인 경우 이 오타와 그에 따른 예외는 컨테이너가 배포된 후 오랜 시간이 지난 후에야 발견될 수 있습니다.

{% hint style="info" %}
`idref` 요소의 로컬 속성은 더 이상 일반 bean 참조에 대한 값을 제공하지 않으므로 4.0 bean XSD에서 더 이상 지원되지 않습니다. 4.0 스키마로 업그레이드할 때 기존 `idref local` 참조를 `idref bean`으로 변경해야 한다.
{% endhint %}

\<idref/> 요소가 값을 가져오는 일반적인 위치(최소한 Spring 2.0 이전 버전에서)는 `ProxyFactoryBean` bean definition의 [AOP Interceptors](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-pfb-1) 구성에 있습니다. 인터셉터 이름을 지정할 때 \<idref/>요소를 사용하면 인터셉터 ID의 오타를 방지할 수 있습니다.



### **References to Other Beans (Collaborators)**

ref 요소는 \<constructor-arg/> 또는 \<property/> 정의 요소 내부의 마지막 요소입니다. 여기에서 빈의 지정된 속성 값을 컨테이너가 관리하는 다른 빈(협력자)에 대한 참조로 설정합니다. 참조된 Bean은 속성을 설정할 Bean의 의존성이며 속성이 설정되기 전에 필요에 따라 초기화됩니다. (협력자가 싱글톤 빈인 경우 이미 컨테이너에 의해 초기화되었을 수 있습니다.). 모든 참조는 궁극적으로 다른 개체에 대한 참조입니다. 범위 지정 및 유효성 검증은 `bean` 또는 `parent` 속성을 통해 다른 오브젝트의 ID 또는 이름을 지정하는지 여부에 따라 다릅니다.



`<ref>` 태그의 `bean` 속성을 통해 대상 bean을 지정하는 것은 가장 일반적인 형식이며 동일한 XML 파일에 있는지 여부에 관계없이 동일한 컨테이너 또는 부모 컨테이너의 모든 bean에 대한 참조를 생성할 수 있습니다. `Bean` 속성의 값은 대상 Bean의 `id` 속성과 동일하거나 대상 Bean의 `name` 속성 값 중 하나와 동일할 수 있습니다. 다음 예제에서는 `ref` 요소를 사용하는 방법을 보여줍니다.

```xml
<ref bean="someBean"/>
```

`parent` 속성을 통해 대상 bean을 지정하면 현재 컨테이너의 상위 컨테이너에 있는 bean에 대한 참조가 생성됩니다. `parent` 속성의 값은 대상 Bean의 id 속성 또는 대상 Bean의 `name` 속성 값 중 하나와 동일할 수 있습니다.  대상 빈은 현재 빈의 상위 컨테이너에 있어야 합니다. 컨테이너의 계층 구조가 있고 부모 빈과 동일한 이름을 가진 프록시를 사용하여 부모 컨테이너의 기존 빈을 래핑하려는 경우 주로 이 빈 참조 변형을 사용해야 합니다. 다음 두가지 내용은 parent 속성을 사용하는 방법을 보여줍니다.

```xml
<!-- 상위 컨텍스트에서 -->
<bean id="accountService" class="com.something.SimpleAccountService">
    <!-- 여기에 필요에 따라 의존성을 삽입합니다. -->
</bean>
```

```xml
<!-- 하위(하위) 컨텍스트에서 -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- 부모 bean을 참조하는 방법에 주목합니다. -->
    </property>
    <!-- 여기에 필요에 따라 다른 구성 및 의존성을 삽입합니다. -->
</bean>
```

{% hint style="info" %}
`ref` 요소의 `local` 속성은 더 이상 일반 bean 참조에 대한 값을 제공하지 않기 때문에 4.0 bean XSD에서 더 이상 지원되지 않습니다. 4.0 스키마로 업그레이드할 때 기존 `ref local` 참조를 `ref bean`으로 변경하십시오.
{% endhint %}



### Inner Beans

`<proerty/>` 또는 `<constructor-arg/>` 요소 내부의 `<bean/>`요소는 다음 예제와 같이 내부 빈을 정의합니다. 다음 예를 볼 수 있듯이:

```xml
<bean id="outer" class="...">
    <!-- 대상 bean에 대한 참조를 사용하는 대신 대상 bean을 인라인으로 간단히 정의합니다 -->
    <property name="target">
        <bean class="com.example.Person"> <!-- 이것이 inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```

inner bean definition에는 정의된 ID나 이름이 필요하지 않습니다. 지정된 경우 컨테이너는 이러한 값을 식별자로 사용하지 않습니다. Inner bean은 항상 익명이고 항상 외부 빈과 함께 생성되기 때문에 컨테이너는 생성 시 `scope` 플래그도 무시합니다. Inner bean에 독립적으로 액세스하거나 Inner bean의 enclosing bean이 아닌 협력 bean에 주입할 수 없습니다.



특수한 경우로 사용자 scope에서 파괴 콜백을 받을 수 있습니다. (예를 들어 요청 범위의 싱글톤 bean에 포함된  inner bean의 경우). 내부 bean 인스턴스의 생성은 포함하는 bean에 연결되어 있지만 파괴 콜백을 통해 요청 범위의 수명 주기에 참여할 수 있습니다. 이것은 일반적인 시나리오가 아닙니다. Inner bean은 일반적으로 단순히 포함하는 빈의 범위를 공유합니다



### Collections

`<list/>`,`<set/>` ,`<map/>` 및 `<props/>` 요소는 각각 Java 컬렉션 유형 List, Set, Map 및 Properties의 속성 및 인수를 설정합니다. 다음 예에서는 사용 방법을 보여줍니다.

```xml
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- setAdminEmails(java.util.Properties) 호출 결과 -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- setSomeList(java.util.List) 호출 결과 -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- setSomeMap(java.util.Map) 호출 결과 -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- setSomeSet(java.util.Set) 호출 결과 -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```

Map key나 value 또는 설정 value의 값은 다음 요소 중 하나일 수도 있습니다

```xml
bean | ref | idref | list | set | map | props | value | null
```



#### Collection Merging

Spring 컨테이너는 컬렉션 병합도 지원합니다. 애플리케이션 개발자는 상위 `<list/>`,`<map/>`,`<set/>` 또는 `<props/>` 요소를 정의하고 상위 컬렉션에서 값을 상속하고 override한 하위 `<list/>`,`<map/>`,`<set/>` 또는 `<props/>` 요소를 가질 수 있습니다. 즉, 하위 컬렉션의 값은 상위 컬렉션과 하위 컬렉션의 요소를 병합한 결과이며 하위 컬렉션 요소는 상위 컬렉션에 지정된 값보다 우선합니다.



병합에 대한 이 섹션에서는 parent-child bean 메커니즘에 대해 설명합니다. Parent and child Bean 정의에 익숙하지 않은 독자는 계속하기 전에 [관련 섹션](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-child-bean-definitions)을 읽어보기를 원할 수 있습니다.



다음 예제에서는 컬렉션 병합을 보여줍니다.

```xml
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- 병합은 하위 컬렉션 정의에 지정됩니다. -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```

자식 빈 정의의 `adminEmails` 속성의 `<props/>`요소에서 `merge=true` 속성의 사용에 주목하세요. `Child` bean이 컨테이너에 의해 확인되고 인스턴스화되면 결과 인스턴스에는 자식의 `adminEmails` 컬렉션을 부모의 `adminEmails` 컬렉션과 병합한 결과가 포함된 `adminEmails Properties` 컬렉션이 있습니다. 다음 목록은 결과를 보여줍니다.

```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```

child `Properties` 컬렉션의 값 집합은 parent \<props/>의 모든 속성 요소를 상속하며 `support` 값에 대한 child 값은 상위 컬렉션의 값을 재정의합니다.



이 병합 동작은 \<list/>,\<map/> 및 \<set/> 컬렉션 유형에 유사하게 적용됩니다. \<list/>요소의 특정한 경우에 List 컬렉션 유형(즉, 정렬된 값 컬렉션의 개념)과 관련된 의미 체계가 유지됩니다. Parent의 값은 child 목록의 모든 값보다 우선합니다. Map, Set 및 Properties 컬렉션 유형의 경우 순서가 없습니다. 따라서 컨테이너가 내부적으로 사용하는 연결된 Map, Set 및 Properties 구현 유형의 기반이 되는 컬렉션 유형에 대해 순서 지정 의미 체계가 적용되지 않습니다.



#### Limitations of Collection Merging

서로 다른 컬렉션 유형(예: `Map` 및 `List`)을 병합할 수 없습니다. 그렇게 하려고 하면 적절한 예외가 발생합니다. `merge` 속성은 하위의 상속된 chiled definition에 지정되어야 합니다. 상위 컬렉션 definition에 `merge` 속성을 지정하는 것은 중복되며 원하는 병합으로 이어지지 않습니다.



#### Strongly-typed collection

generic types 대한 Java의 지원 덕분에 강력한 타입의 collections을 사용할 수 있습니다. 즉, (예를 들어) `String` 요소만 포함할 수 있도록 `Collection` 타입을 선언할 수 있습니다.  `Spring`을 사용하여 강력한 타입의 Collection을 bean에 의존성 주입하는 경우 강력한 타입의 `Collection` 인스턴스 요소가 `Collection`에 추가되기 전에 적절한 타입으로 변환되도록 `Spring`의 타입 변환 지원을 활용할 수 있습니다. 다음 Java 클래스 및 bean definition는 이를 수행하는 방법을 보여줍니다.

```java
public class SomeClass {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```

```xml
<beans>
    <bean id="something" class="x.y.SomeClass">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```

`something` 빈의 `accounts` 속성이 주입을 위해 준비되면 강력한 타입의 `Map<String, Float>` 요소 유형에 대한 제네릭 정보를 리플렉션을 통해 사용할 수 있습니다. 따라서 Spring의 타입 변환 인프라는 다양한 값 요소를 Float 유형으로 인식하고 문자열 값(9.99, 2.75 및 3.99)을 실제 Float 유형으로 변환합니다.



### **Null and Empty String Values**

Spring은 속성 등에 대한 빈 인수를 빈 `Strings`로 취급합니다. 다음 XML 기반 configuration metadata 조각은 `email` 속성을 빈 `String` 값("")으로 설정합니다.

```xml
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

앞의 예제는 다음 Java 코드와 동일합니다

```java
exampleBean.setEmail("");
```

\<null/>요소는 null 값을 처리합니다. 다음 목록은 예를 보여줍니다.

```xml
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```

앞의 구성은 다음 Java 코드와 동일합니다

```java
exampleBean.setEmail(null);
```



### **XML Shortcut with the p-namespace**

p-namespace를 사용하면 `bean` 요소의 속성(중첩된 `<property/>`요소 대신)을 사용하여 속성 값,협업 빈 또는 둘 다 설명할 수 있습니다.&#x20;



Spring은 XML 스키마 definition를 기반으로 하는 [namespaces](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#core.appendix.xsd-schemas)로 확장 가능한 구성 형식을 지원합니다. 이 장에서 설명하는 `beans` 구성 형식은 XML 스키마 문서에 정의되어 있습니다. 그러나 p-namespace는 XSD 파일에 정의되어 있지 않으며 Spring의 코어에만 존재합니다.



다음 예는 동일한 결과로 확인되는 두 개의 XML 스니펫(첫 번째는 표준 XML 형식을 사용하고 두 번째는 p-namespace를 사용함)을 보여줍니다.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>

    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
</beans>
```

예제는 bean definition에서 `email`이라는 p-namespace의 속성을 보여줍니다. 이것은 속성 선언을 포함하도록 Spring에 지시합니다. 앞에서 언급했듯이 p-namespace에는 스키마 정의가 없으므로 특성 이름을 속성 이름으로 설정할 수 있습니다.



이 다음 예제는 다른 bean에 대한 참조가 있는 두 개의 추가 bean 정의를 포함합니다.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```

이 예제에는 p-namespace를 사용하는 속성 값뿐만 아니라 속성 참조를 선언하는 특수 형식도 사용됩니다. 첫 번째 bean definition는 `<property name="spouse" ref="jane"/>`을 사용하여 bean `john`에서 bean `jane`으로의 참조를 생성하는 반면 두 번째 bean definition는 `p:spouse-ref="jane"`를 속성으로 사용하여 똑같은 일을 수행합니다. 이 경우 `spouse`는 속성 이름인 반면 `-ref` 부분은 이것이 적접 값이 아니라 다른 빈에 대한 참조임을 나타냅니다.

{% hint style="info" %}
p-namespace은 표준 XML 형식만큼 유연하지 않습니다. 예를 들어 속성 ​​참조를 선언하는 형식은 `Ref`로 끝나는 속성과 충돌하지만 표준 XML 형식은 그렇지 않습니다. 세 가지 접근 방식을 모두 동시에 사용하는 XML 문서를 생성하지 않도록 접근 방식을 신중하게 선택하고 이를 팀원에게 전달하는 것이 좋습니다.
{% endhint %}



### **XML Shortcut with the c-namespace**

[XML shortcut with the p-namespace](ioccontainer.md#xml-shortcut-with-the-p-namespace)를 비스한 Spring 3.1에 도입된 c-네임스페이스는 중첩된 `constructor-arg` 요소가 아닌 생성자 인수를 구성하기 위한 인라인 속성을 허용합니다.



다음 예제에서는 **c:** namespace 사용하여 from [Constructor-based Dependency Injection](ioccontainer.md#constructor-based-dependency-injection)과 동일한 작업을 수행합니다.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beanTwo" class="x.y.ThingTwo"/>
    <bean id="beanThree" class="x.y.ThingThree"/>

    <!-- 선택적 인수 이름이 있는 전통적인 선언 -->
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg name="thingTwo" ref="beanTwo"/>
        <constructor-arg name="thingThree" ref="beanThree"/>
        <constructor-arg name="email" value="something@somewhere.com"/>
    </bean>

    <!-- 인수 이름이 있는 c-namespace 선언 -->
    <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
        c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>

</beans>
```

`c:` namespace 이름으로 생성자 인수를 설정하기 위해 `p:`(bean 참조에 대한 후행 `-ref`)과 동일한 규칙을 사용합니다. 마찬가지로 XSD 스키마에 정의되어 있지 않더라도 XML 파일에 선언해야 합니다(스프링 코어 내부에 존재).



드물게 생성자 인수 이름을 사용할 수 없는 경우(일반적으로 바이트코드가 디버깅 정보 없이 컴파일된 경우) 다음과 같이 인수 인덱스에 대한 fallback을 사용할 수 있습니다.

```xml
<!-- c-namespace index declaration -->
<bean id="beanOne" class="x.y.ThingOne" c:_0-ref="beanTwo" c:_1-ref="beanThree"
    c:_2="something@somewhere.com"/>
```

{% hint style="info" %}
XML 문법으로 인해 index 표기법에는 XML 속성 이름이 숫자로 시작할 수 없기 때문에(일부 IDE에서 허용하더라도) 앞에 \_가 있어야 합니다. 해당 index 표기법은 `<constructor-arg>` 요소에도 사용할 수 있지만 일반적으로 일반 선언 순서로 충분하기 때문에 일반적으로 사용되지 않습니다.
{% endhint %}



실제로 [constructor resolution mechanism](ioccontainer.md#constructor-argument-resolution)은 인수를 매칭시키는 데 매우 효율적이므로 실제로 필요한 경우가 아니면 구성 전체에서 이름 표기법을 사용하는 것이 좋습니다.



### **Compound Property Names**

최종 속성 이름을 제외한 경로의 모든 구성 요소가 `null`이 아닌 한 빈 속성을 설정할 때 복합 또는 중첩 속성 이름을 사용할 수 있습니다. 다음 bean definition를 고려합니다.

```xml
<bean id="something" class="things.ThingOne">
    <property name="fred.bob.sammy" value="123" />
</bean>
```

`Something` 빈은 `fred` 속성을 가지고 있습니다. `bob` 속성은 `sammy` 속성을 가지고 있고 최종 `sammy` 속성은 값 `123`으로 설정됩니다. 이것이 작동하려면 무언가의 `fred` 속성과 `fred`의 `bob` 속성이 `bean`이 생성된 후 `null`이 아니어야 합니다. 그렇지 않으면 `NullPointerException`이 발생합니다.



## 4.3 Using depends-on



빈이 다른 빈의 의존성인 경우 일반적으로 한 빈이 다른 빈의 속성으로 설정됨을 의미합니다. 일반적으로 XML 기반 metadata의 [\<ref/> element](ioccontainer.md#references-to-other-beans-collaborators)하여 이를 수행합니다. 그러나 때로는 빈 간의 의존성이 덜 직접적입니다. 데이터베이스 드라이버 등록과 같이 클래스의 정적 초기화 프로그램을 트리거해야 하는 경우를 예로 들 수 있습니다. `depends-on` 속성은 이 요소를 사용하는 bean이 초기화되기 전에 초기화될 하나 이상의 bean을 명시적으로 강제할 수 있습니다. 다음 예제에서는 `depends-on` 속성을 사용하여 단일 빈에 대한 의존성을 표현합니다.

```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />
```

여러 bean에 대한 의존성을 표현하려면 depends-on 속성의 값으로 bean 이름 list을 제공합니다(쉼표, 공백 및 세미콜론은 유효한 구분 기호입니다).

```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
```

{% hint style="info" %}
`depends-on` 속성은 초기화 타임의 의존성과 또 싱글톤 빈의 경우 해당 파괴 타임 의존성을 모두 지정할 수 있습니다. 주어진 bean과 `depends-on`를 정의하는 종속 bean은 주어진 bean 자체가 파괴되기 전에 먼저 파괴됩니다. 따라서 `depends-on`은 종료 순서도 제어할 수 있습니다.
{% endhint %}



## 4.4 Lazy-initialized Beans

기본적으로 `ApplicationContext` 구현은 초기화 프로세스의 일부로 모든 [싱글톤](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton) 빈을 열심히 만들고 구성합니다. 일반적으로 이러한 사전 인스턴스화는 구성 또는 주변 환경의 오류가 몇 시간 또는 며칠 후에 발견되는 것이 아니라 즉시 발견되기 때문에 바람직합니다. 이 동작이 바람직하지 않은 경우 bean definition을 지연 초기화로 표시하여 싱글톤 bean의 미리 인스턴스화를 방지할 수 있습니다. Lazy-initialized 빈은 IoC 컨테이너가 시작할 때가 아니라 처음 요청될 때 빈 인스턴스를 생성하도록 지시합니다.



XML에서 이 동작은 다음 예제와 같이 `<bean/>`요소의 `lazy-init` 속성에 의해 제어됩니다.

```xml
<bean id="lazy" class="com.something.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.something.AnotherBean"/>
```



앞의 구성이 `ApplicationContext`에 의해 소비될 때 `lazy` 빈은 `ApplicationContext`가 시작될 때 열심히 미리 인스턴스화되지 않는 반면 `not.lazy` 빈은 열심히 미리 인스턴스화됩니다.



그러나 lazy-initialized된 빈이 지연 초기화되지 않은 싱글톤 빈의 의존성인 경우 `ApplicationContext`는 시작 시 지연 초기화된 빈을 생성합니다. 왜냐하면 싱글톤의 의존성을 충족해야 하기 때문입니다. lazy-initialized bean은 not lazy-initialized 다른 곳의 싱글톤 bean에 주입됩니다.



다음 예제와 같이 `<beans/>`요소의 `default-lazy-init` 속성을 사용하여 컨테이너 수준에서 lazy-initialization를 제어할 수도 있습니다.

```xml
<beans default-lazy-init="true">
    <!-- 어떤 빈도 미리 인스턴스화되지 않습니다.... -->
</beans>
```



## 4.5 Autowiring Collaborators



Spring 컨테이너는 협업 빈 사이의 관계를 자동으로 연결할 수 있습니다. Spring이 `ApplicationContext`의 내용을 검사하여 빈에 대해 자동으로 협력자(다른 빈)를 해결하도록 할 수 있습니다. Autowiring에는 다음과 같은 이점이 있습니다.

* Autowiring은 속성 또는 생성자 인수를 지정해야 하는 필요성을 크게 줄일 수 있습니다. ([이 장의 다른 곳](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-child-bean-definitions)에서 논의된 bean 템플릿과 같은 다른 메커니즘도 이와 관련하여 가치가 있습니다.)
* Autowiring은 객체가 개선함에 따라 구성을 업데이트할 수 있습니다. 예를 들어 클래스에 의존성을 추가해야 하는 경우 구성을 수정하지 않고도 해당 의존성을 자동으로 충족할 수 있습니다. 따라서 autowiring은 코드 베이스가 더 안정될 때 명시적 연결로 전환하는 옵션을 부정하지 않아 개발 중에 특히 유용할 수 있습니다.



XML 기반 configuration metadat([Dependency Injection](ioccontainer.md#4.1-dependency-injection) 참조)를 사용할 때 `<bean/>`요소의 `autowire` 속성을 사용하여 빈 정의에 대한 autowire 모드를 지정할 수 있습니다. 자동 연결 기능에는 네 가지 모드가 있습니다. bean별로 autowiring을 지정하고 따라서 autowiring할 항목을 선택할 수 있습니다. 다음 표는 네 가지 자동 연결 모드를 설명합니다.



_Table 2. Autowiring modes_

<table><thead><tr><th width="162.33333333333331">Mode</th><th>설명</th></tr></thead><tbody><tr><td><code>no</code></td><td>(기본값) 자동 연결 없음. Bean 참조는 <code>ref</code> 요소에 의해 정의되어야 합니다. 공동 작업자를 명시적으로 지정하면 더 많은 제어와 명확성을 제공하므로 대규모 배포에는 기본 설정을 변경하지 않는 것이 좋습니다. 어느 정도까지는 시스템의 구조를 문서화합니다.</td></tr><tr><td><code>byName</code></td><td>속성 이름으로 Autowiring. Spring은 autowired가 필요한 프로퍼티와 같은 이름을 가진 빈을 찾습니다. 예를 들어 bean definition가 이름으로 autowire로 설정되고 <code>master</code> 속성(즉, <code>setMaster(..)</code> 메서드가 있음)을 포함하는 경우 Spring은 <code>master</code>라는 bean definition를 찾고 속성을 설정하는 데 사용합니다.</td></tr><tr><td><code>byType</code></td><td>컨테이너에 속성 타입이 정확히 하나의 빈이  존재하는 경우 속성이 자동 연결되도록 합니다. 둘 이상이 존재하면 치명적인 예외가 발생하여 해당 빈에 대해 <code>byType</code> autowiring을 사용할 수 없게 된다. 일치하는 빈이 없으면 아무 일도 일어나지 않습니다(속성이 설정되지 않음).</td></tr><tr><td><code>constructor</code></td><td><code>byType</code>과 유사하지만 생성자 인수에 적용됩니다. 컨테이너에 생성자 인수 타입의 빈이 정확히 하나도 없으면 치명적인 오류가 발생합니다.</td></tr></tbody></table>

`byType` 또는 `constructor` 자동 연결 모드를 사용하면 배열 및 타입이 지정된 컬렉션을 연결할 수 있습니다.  이러한 경우 예상 타입과 일치하는 컨테이너 내의 모든 autowire 후보가 의존성을 충족하도록 제공됩니다. 예상되는 키 타입이 `String`인 경우 강력한 타입의 `map` 인스턴스를 자동으로 연결할 수 있습니다. autowired `map` 인스턴스의 값은 예상되는 타입과 일치하는 모든 빈 인스턴스로 구성되며 `map` 인스턴스의 키에는 해당 빈 이름이 포함됩니다



**Limitations and Disadvantages of Autowiring**

Autowiring은 프로젝트 전체에서 일관되게 사용될 때 가장 잘 작동합니다. autowiring이 일반적으로 사용되지 않는 경우 한두 개의 빈 정의만 연결하는 데 사용하는 것이 개발자가 혼란스러울 수 있습니다.



autowiring의 한계와 단점:

* `property` 및 `constructor-arg` 설정의 명시적 의존성은 항상 autowiring을 재정의합니다. `primitives`, `Strings`  및 `Classes`(및 이러한 단순 속성의 `arrays`)와 같은 단순 속성을 자동으로 연결할 수 없습니다.이 제한은 의도적으로 설계된 것입니다
* 자동 연결은 명시적 연결보다 덜 정확합니다. 앞의 표에서 언급한 것처럼 Spring은 예기치 않은 결과가 발생할 수 있는 모호한 경우 추측을 피하도록 주의합니다. Spring 관리 객체 간의 관계는 더 이상 명시적으로 문서화되지 않습니다.
* Spring 컨테이너에서 문서를 생성할 수 있는 도구에는 연결 정보가 제공되지 않을 수 있습니다.
* 컨테이너 내의 여러 bean definations는 autowired될 setter 메서드 또는 생성자 인수에 의해 지정된 타입과 매칭할 수 있습니다. 배열, 컬렉션 또는 `map` 인스턴스의 경우 이것이 반드시 문제가 되는 것은 아닙니다. 그러나 단일 값을 예상하는 의존성의 경우 이 모호성이 독단적으로 해결되지 않습니다. 고유한 bean definition를 사용할 수 없으면 예외가 발생합니다.



후자의 시나리오에는 다음과 같은 몇 가지 옵션이 있습니다.

* explicit wiring을 위해 autowiring을 포기합니다.
* [다음 섹션](ioccontainer.md#excluding-a-bean-from-autowiring)에서 설명하는 것처럼 `autowire-candidate` 속성을 `false`로 설정하여 bean definition에 대한 자동 연결을 피합니다.
* `<bean/>`요소의 기본 속성을 `true`로 설정하여 단일 bean definition를 `primary` 후보로 지정합니다.
* [Annotation-based 컨테이너 구성](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config)에 설명된 대로 annotation-based 구성으로 보다 세분화된 구현을 제어 가능합니다.



### **Excluding a Bean from Autowiring**

Bean 단위로 autowiring에서 bean을 제외할 수 있습니다. Spring의 XML 형식에서 `<bean/>`요소의 `autowire-candidate` 속성을 `false`로 설정합니다. 컨테이너는 특정 bean definition을 자동 연결 인프라([@Autowired](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-autowired-annotation)와 같은 annotation 스타일 구성 포함)에서 사용할 수 없게 만듭니다.



{% hint style="info" %}
`Autowire-candidate` 속성은 타입 기반 autowiring에만 영향을 미치도록 설계되었습니다. 지정된 bean이 autowire 후보로 표시되지 않더라도 해결되는 이름에 의한 명시적 참조에는 영향을 미치지 않습니다. 결과적으로 이름에 의한 autowiring은 그럼에도 불구하고 이름이 일치하면 bean을 주입합니다.
{% endhint %}

또한 bean 이름에 대한 패턴 일치를 기반으로 autowire 후보를 제한할 수 있습니다. 최상위 `<beans/>`요소는 `default-autowire-candidates` 속성 내에서 하나 이상의 패턴을 허용합니다. 예를 들어 이름이 `Repository`로 끝나는 빈으로 autowire 후보 상태를 제한하려면 `*Repository` 값을 제공합니다. 여러 패턴을 제공하려면 쉼표로 구분된 list으로 정의합니다. Bean definition의 `autowire-candidate` 속성에 대한 `true` 또는 `false`의 명시적 값이 항상 우선합니다. 이러한 빈의 경우 패턴 일치 규칙이 적용되지 않습니다.



이러한 기술은 autowiring에 의해 다른 bean에 주입되고 싶지 않은 bean에 유용합니다. autowiring을 사용하여 제외된 bean 자체를 구성할 수 없다는 의미는 아닙니다. bean 자체가 다른 bean을 autowiring하기 위한 후보가 아니라는 의미입니다.



## **4.6. Method Injection**

대부분의 애플리케이션 시나리오에서 컨테이너의 대부분의 빈은 [싱글톤](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton)입니다. 싱글톤 bean이 다른 싱글톤 bean과 협업해야 하거나 비 싱글톤 bean이 다른 비 싱글톤 bean과 협업해야 하는 경우 일반적으로 한 bean을 다른 bean의 속성으로 정의하여 의존성을 처리합니다. 빈 라이프사이클이 다를 때 문제가 발생합니다. 싱글톤 빈 A가 싱글톤이 아닌(프로토타입) 빈 B를 사용해야 한다고 가정하자. 컨테이너는 싱글톤 빈 A를 한 번만 생성하므로 속성을 설정할 수 있는 기회는 한 번뿐입니다. 컨테이너는 필요할 때마다 Bean B의 새 인스턴스를 Bean A에 제공할 수 없습니다.



해결책은 제어 역전을 포기하는 것입니다.`ApplicationContextAware` 인터페이스를 구현하고 [컨테이너에 대한 getBean("B") 호출](ioccontainer.md#2.3)을 수행하여 bean A가 필요할 때마다 (일반적으로 새로운) bean B 인스턴스를 요청함으로써 [bean A가 컨테이너를 인식](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-aware)하도록 할 수 있습니다. 다음 예에서는 이 접근 방식을 보여줍니다.

```java
package fiona.apple;

// Spring-API imports
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

/**
 * 일부 처리를 수행하기 위해 상태 저장 명령 스타일 클래스를 사용하는 클래스
 */
public class CommandManager implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // 적절한 명령의 새 인스턴스를 가져옵니다.
        Command command = createCommand();
        // (완전히 새로운) 명령 인스턴스에 상태를 설정합니다
        command.setState(commandState);
        return command.execute();
    }

    protected Command createCommand() {
        // Spring API 종속성을 확인하세요!
        return this.applicationContext.getBean("command", Command.class);
    }

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

비즈니스 코드가 Spring Framework를 인식하고 결합하기 때문에 앞의 내용은 바람직하지 않습니다. Spring IoC 컨테이너의 고급 기능인 메서드 주입을 사용하면 이 사용 사례를 깔끔하게 처리할 수 있습니다.

{% hint style="info" %}
이 [블로그 항목](https://spring.io/blog/2004/08/06/method-injection)에서 메소드 주입의 동기에 대해 자세히 읽을 수 있습니다.
{% endhint %}

### **Lookup Method Injection**

조회 메소드 주입은 컨테이너가 관리하는 Bean의 메서드를 대체하고 컨테이너의 다른 명명된 Bean에 대한 조회 결과를 리턴하는 컨테이너의 기능입니다. 조회에는 [이전 섹션](ioccontainer.md#4.6.-method-injection)에서 설명한 시나리오에서와 같이 일반적으로 프로토타입 빈이 포함됩니다. Spring Framework는 메서드를 재정의하는 하위 클래스를 동적으로 생성하기 위해 CGLIB 라이브러리의 바이트 코드 생성을 사용하여 이 메서드 주입을 구현합니다.



{% hint style="info" %}
* 이 동적 하위 클래스가 작동하려면 Spring Bean 컨테이너 하위 클래스가 `final` 클래스가 될 수 없으며 재정의되는 메서드도 `final` 클래스가 될 수 없습니다.
* `abstract` 메서드가 있는 클래스를 단위 테스트하려면 클래스를 직접 하위 클래스로 만들고 `abstract` 메서드의 스텁 구현을 제공해야 합니다.
* 구체적인 클래스를 선택해야 하는 구성 요소 스캔에도 구체적인 메서드가 필요합니다.
* 추가 주요 제한 사항은 조회 메서드가 팩토리 메서드, 특히 구성 클래스의 `@Bean` 메서드와 함께 작동하지 않는다는 것입니다. 이 경우 컨테이너는 인스턴스 생성을 담당하지 않으므로 런타임 생성 하위 클래스를 즉시 생성할 수 없습니다.
{% endhint %}



이전 코드 스니펫의 `CommandManager` 클래스의 경우 Spring 컨테이너는 `createCommand()` 메서드의 구현을 동적으로 재정의합니다. 재작업된 예제에서 볼 수 있듯이 CommandManager 클래스에는 Spring 종속성이 없습니다.

```java
package fiona.apple;

// no more Spring imports!

public abstract class CommandManager {

    public Object process(Object commandState) {
        // 적절한 명령 인터페이스의 새 인스턴스를 가져옵니다.
        Command command = createCommand();
        // (완전히 새로운) 명령 인스턴스에 상태를 설정합니다.
        command.setState(commandState);
        return command.execute();
    }

    // 알겠습니다... 하지만 이 방법의 구현은 어디에 있습니까?
    protected abstract Command createCommand();
}
```

주입할 메서드(이 경우 `CommandManager`)가 포함된 클라이언트 클래스에서 주입할 메서드에는 다음 형식의 서명이 필요합니다.

```xml
<public|protected> [abstract] <return-type> theMethodName(no-arguments);
```

메서드가 `abstract`이면 동적으로 생성된 하위 클래스가 메서드를 구현합니다. 그렇지 않으면 동적으로 생성된 하위 클래스가 원래 클래스에 정의된 구체적인 메서드를 재정의합니다. 다음 예를 고려합니다.

```xml
<!-- 프로토타입으로 배포된 상태 저장 빈(비싱글톤) -->
<bean id="myCommand" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- 필요에 따라 여기에 의존성을 주입합니다. -->
</bean>

<!-- commandProcessor는 statefulCommandHelper를 사용합니다. -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="myCommand"/>
</bean>
```

`commandManager`로 식별된 빈인 `myCommand` 빈의 새 인스턴스가 필요할 때마다 자신의 `createCommand()`메서드를 호출합니다. 실제로 필요한 경우 프로토타입으로 `myCommand` 빈을 배치하는 데 주의해야 합니다. [싱글톤](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton)이면 매번 동일한 `myCommand` 빈 인스턴스가 반환됩니다.&#x20;



또는 annotation-based 구성 요소 모델 내에서 다음 예제와 같이 @Lookup annotation을 통해 조회 메서드를 선언할 수 있습니다.

```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup("myCommand")
    protected abstract Command createCommand();
}
```

또는 보다 관용적으로 조회 메서드의 선언된 반환 유형에 대해 해결되는 대상 bean에 의존할 수 있습니다.

```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup
    protected abstract Command createCommand();
}
```

추상 클래스가 기본적으로 무시되는 Spring의 구성 요소 검색 규칙과 호환되도록 하려면 일반적으로 구체적인 스텁 구현으로 annotation이 달린 조회 메서드를 선언해야 합니다. 이 제한은 명시적으로 등록되거나 명시적으로 가져온 Bean 클래스에는 적용되지 않습니다.



{% hint style="info" %}
다른 범위의 대상 bean에 액세스하는 또 다른 방법은 `ObjectFactory`/`Provider` 주입 지점입니다. [Scoped Beans as Dependencies](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-other-injection)를 참조합니다.



`ServiceLocatorFactoryBean`(`org.springframework.beans.factory.config` 패키지에 있음)을 찾아보면 도움이 될수 있습니다.
{% endhint %}



### **Arbitrary Method Replacement**

조회 메서드 주입보다 덜 유용한 메서드 주입 형식은 관리 빈의 임의 메서드를 다른 메서드 구현으로 대체하는 기능입니다. 이 기능이 실제로 필요할 때까지 이 섹션의 나머지 부분을 안전하게 건너뛸 수 있습니다.



XML 기반 configuration metadata를 사용하면 `replaced-method` 요소를 사용하여 배치된 Bean에 대해 기존 메소드 구현을 다른 구현으로 대체할 수 있습니다.  재정의하려는 `computeValue`라는 메서드가 있는 다음 클래스를 고려합니다.

```java
public class MyValueCalculator {

    public String computeValue(String input) {
        // some 실제 코드...
    }

    // some other methods...
}
```



`org.springframework.beans.factory.support.MethodReplacer` 인터페이스를 구현하는 클래스는 다음 예제와 같이 새로운 메서드 정의를 제공합니다.



```java
/**
 * MyValueCalculator에서 기존 computeValue(String) 구현을 재정의하는 데 사용됩니다.
 */
public class ReplacementComputeValue implements MethodReplacer {

    public Object reimplement(Object o, Method m, Object[] args) throws Throwable {
        // 입력 값을 가져와 작업하고 계산된 결과를 반환합니다.
        String input = (String) args[0];
        ...
        return ...;
    }
}
```

원래 클래스를 배포하고 메서드 재정의를 지정하는 bean definition는 다음 예제와 유사합니다.

```xml
<bean id="myValueCalculator" class="x.y.z.MyValueCalculator">
    <!-- arbitrary method replacement -->
    <replaced-method name="computeValue" replacer="replacementComputeValue">
        <arg-type>String</arg-type>
    </replaced-method>
</bean>

<bean id="replacementComputeValue" class="a.b.c.ReplacementComputeValue"/>
```

`<replaced-method/>`요소 내에서 하나 이상의 `<arg-type/>` 요소를 사용하여 재정의되는 메서드의 메서드 시그니처를 나타낼 수 있습니다. 인수에 대한 서명은 메서드가 오버로드되고 클래스 내에 여러 변형이 있는 경우에만 필요합니다. 편의를 위해 인수의 유형 문자열은 정규화된 유형 이름의 하위 문자열일 수 있습니다. 예를 들어 다음은 모두 `java.lang.String`과 일치합니다.

```java
java.lang.String
String
Str
```

인수의 수는 가능한 각 선택 항목을 구별하기에 충분하기 때문에 이 단축키를 사용하면 인수 유형과 일치하는 가장 짧은 문자열만 입력할 수 있으므로 입력 시간을 많이 절약할 수 있습니다.

## 5. Bean Scopes

Bean definition를 생성할 때 해당 bean definition에 의해 정의된 클래스의 실제 인스턴스를 생성하기 위한 레시피를 생성합니다. Bean definition가 하나의 레시피라는 생각은 중요하다. 클래스와 마찬가지로 하나의 레시피에서 많은 객체 인스턴스를 생성할 수 있다는 의미이기 때문이다.



특정 bean definition에서 생성된 객체에 연결되는 다양한 의존성 및 구성 값을 제어할 수 있을 뿐만 아니라 특정 bean definition에서 생성된 객체의 범위를 제어할 수 있습니다. 이 접근 방식은 강력하고 유연합니다. Java 클래스 수준에서 객체의 범위를 정하는 대신 구성을 통해 생성하는 객체의 범위를 선택할 수 있기 때문입니다. Spring Framework는 6개의 범위를 지원하며 그 중 4개는 웹 인식 `ApplicationContext`를 사용하는 경우에만 사용할 수 있습니다. [사용자 지정 범위](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-custom)를 만들 수도 있습니다.



다음 표에는 지원되는 범위을 설명합니다.

<table><thead><tr><th width="176">Scopte</th><th>Description</th></tr></thead><tbody><tr><td>singleton</td><td>(기본값) 각 Spring IoC 컨테이너에 대한 단일 객체 인스턴스에 단일 bean definition의 범위를 지정합니다.</td></tr><tr><td>prototype</td><td>단일 bean definition를 여러 객체 인스턴스로 범위 지정합니다.</td></tr><tr><td>request</td><td>단일 HTTP 요청의 라이프사이클에 대한 단일 bean definition의 범위를 지정합니다. 즉, 각 HTTP 요청에는 단일 bean definition 뒤에서 생성된 자체 bean 인스턴스가 있습니다. 웹 인식 Spring <code>ApplicationContext</code>의 컨텍스트에서만 유효합니다.</td></tr><tr><td>session</td><td>HTTP 세션의 수명 주기에 대한 단일 bean definition의 범위를 지정합니다. 웹 인식 Spring ApplicationContext의 컨텍스트에서만 유효합니다.</td></tr><tr><td>application</td><td>단일 bean definition 범위를 <code>ServletContext</code>의 수명 주기로 지정합니다. 웹 인식 Spring ApplicationContext의 컨텍스트에서만 유효합니다.</td></tr><tr><td>websocket</td><td><code>WebSocket</code>의 수명 주기에 대한 단일 bean definition의 범위를 지정합니다. 웹 인식 Spring ApplicationContext의 컨텍스트에서만 유효합니다.</td></tr></tbody></table>



{% hint style="info" %}
스레드 범위를 사용할 수 있지만 기본적으로 등록되지 않습니다. 자세한 내용은 [`SimpleThreadScope`](https://docs.spring.io/spring-framework/docs/6.0.8/javadoc-api/org/springframework/context/support/SimpleThreadScope.html) 설명서를 참조하십시오. 이 범위 또는 다른 사용자 지정 범위를 등록하는 방법에 대한 지침은 [`사용자 지정 범위 사용`](https://docs.spring.io/spring-framework/docs/6.0.8/javadoc-api/org/springframework/context/support/SimpleThreadScope.html)을 참조하십시오.
{% endhint %}

## 5.1 The Singleton Scope

싱글톤 빈의 하나의 공유 인스턴스만 관리되며 해당 빈 정의와 일치하는 ID를 가진 빈에 대한 모든 요청은 Spring 컨테이너에 의해 반환되는 특정 빈 인스턴스 하나를 초래합니다.



다르게 표현하자면, bean definition를 정의하고 그것이 싱글톤으로 범위가 지정되면 Spring IoC 컨테이너는 해당 bean definition에 의해 정의된 객체의 정확히 하나의 인스턴스를 생성합니다. 이 단일 인스턴스는 이러한 싱글톤 빈의 캐시에 저장되며 해당 명명된 빈에 대한 모든 후속 요청 및 참조는 캐시된 객체를 반환합니다. 다음 이미지는 싱글톤 범위의 작동 방식을 보여줍니다.

<figure><img src="../../../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

Spring의 싱글톤 빈 개념은 Gang of Four(GoF) 패턴 책에 정의된 싱글톤 패턴과 다릅니다. GoF 싱글톤은 특정 클래스의 인스턴스가 ClassLoader당 하나만 생성되도록 개체의 범위를 하드 코딩합니다.Spring 싱글톤의 범위는 per-container 및 per-bean으로 가장 잘 설명됩니다. 즉, 단일 Spring 컨테이너에서 특정 클래스에 대해 하나의 빈을 정의하면 Spring 컨테이너는 해당 bean definition에 의해 정의된 클래스의 인스턴스를 하나만 생성합니다. 싱글톤 범위는 Spring의 기본 범위입니다. XML에서 bean을 싱글톤으로 정의하려면 다음 예제와 같이 bean을 정의할 수 있습니다.

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- 다음은 중복되지만 동일합니다(단일 범위가 기본값임) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

## 5.2 **The Prototype Scope**

비싱글톤 프로토타입 범위의 bean 배치는 특정 bean에 대한 요청이 있을 때마다 새로운 bean 인스턴스를 생성합니다. 즉, 빈이 다른 빈에 주입되거나 컨테이너에서 `getBean()` 메서드 호출을 통해 요청합니다. 일반적으로 모든 stateful bean에는 프로토타입 범위를 사용하고 stateless bean에는 싱글톤 범위를 사용해야 합니다.



다음 다이어그램은 Spring 프로토타입 범위를 보여줍니다.

<figure><img src="../../../../.gitbook/assets/image (6) (2).png" alt=""><figcaption></figcaption></figure>

(DAO(Data Access Object)는 일반적으로 프로토타입으로 구성되지 않습니다. 일반적인 DAO에는 대화 상태가 없기 때문입니다. 싱글톤 다이어그램의 핵심을 재사용하는 것이 더 쉬웠습니다.)



다음 예제는 Bean을 XML의 프로토타입으로 정의합니다.

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

다른 범위와 달리 Spring은 프로토타입 빈의 전체 수명 주기를 관리하지 않습니다. 컨테이너는 프로토타입 객체를 인스턴스화, 구성 및 어셈블하여 해당 프로토타입 인스턴스에 대한 추가 기록 없이 클라이언트에 전달합니다. 따라서 범위에 관계없이 모든 객체에 대해 초기화 생명주기 콜백 메서드가 호출되더라도 프로토타입의 경우에는 설정된 소멸 생명주기 콜백이 호출되지 않습니다. 클라이언트 코드는 프로토타입 범위 객체를 정리하고 프로토타입 빈이 보유하고 있는 값비싼 리소스를 해제해야 합니다. Spring 컨테이너가 프로토타입 범위의 bean이 보유한 리소스를 해제하려면 정리해야 하는 bean에 대한 참조를 보유하는 custom [bean post-processor](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-extension-bpp)를 사용해야 한다.



어떤 면에서 프로토타입 범위 빈에 대한 Spring 컨테이너의 역할은 Java new 연산자를 대체하는 것입니다. 해당 시점 이후의 모든 수명 주기 관리는 클라이언트에서 처리해야 합니다. (Spring 컨테이너에 있는 빈의 수명 주기에 대한 자세한 내용은 수명 [Lifecycle Callbacks](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle)을 참조하세요.)

## 5.3 **Singleton Beans with Prototype-bean Dependencies**

프로토타입 Bean에 대한 종속성이 있는 싱글톤 범위 Bean을 사용하는 경우 인스턴스화 시간에 의존성이 해결된다는 점에 유의하여야 합니다. 따라서 프로토타입 범위의 빈을 싱글톤 범위의 빈에 의존성 주입하면 새 프로토타입 빈이 인스턴스화된 다음 싱글톤 빈에 의존성 주입됩니다. 프로토타입 인스턴스는 싱글톤 범위의 Bean에 제공되는 유일한 인스턴스입니다.&#x20;



그러나 싱글톤 범위의 빈이 런타임 시 반복적으로 프로토타입 범위의 빈의 새 인스턴스를 획득하기를 원한다고 가정한다면 스프링 컨테이너가 싱글톤 빈을 인스턴스화하고 의존성을 해결하고 주입할 때 주입이 한 번만 발생하기 때문에 프로토타입 범위의 빈을 싱글톤 빈에 의존성 주입할 수 없습니다. 런타임 시 프로토타입 빈의 새 인스턴스가 두 번 이상 필요한 경우 [Method Injection](ioccontainer.md#4.6.-method-injection)을 참조하십시오.



## 5.4 **Request, Session, Application, and WebSocket Scopes**

`request`, `session`, `application` 및 `websocket` 범위는 웹 인식 Spring ApplicationContext 구현(예: `XmlWebApplicationContext`)을 사용하는 경우에만 사용할 수 있습니다. `ClassPathXmlApplicationContext`와 같은 일반 Spring IoC 컨테이너와 함께 이러한 범위를 사용하는 경우 알 수 없는 빈 범위에 대해 `IllegalStateException`이 발생합니다.



### **Initial Web Configuration**

`request`, `session`, `application` 및 `websocket` levels(웹-범위 빈)에서 빈 범위 지정을 지원하려면 빈을 정의하기 전에 약간의 초기 구성이 필요합니다. (이 초기 설정은 표준 범위(싱글톤 및 프로토타입)에는 필요하지 않습니다.)



이 초기 설정을 수행하는 방법은 특정 서블릿 환경에 따라 다릅니다.



실제로 Spring `DispatcherServlet`에 의해 처리되는 요청 내에서 Spring Web MVC 내에서 범위가 지정된 bean에 액세스하는 경우 특별한 설정이 필요하지 않습니다.  `DispatcherServlet`은 이미 모든 관련 상태를 exposes합니다.



Spring의 `DispatcherServlet` 외부에서 요청을 처리하는 Servlet 웹 컨테이너를 사용하는 경우(예: JSF를 사용하는 경우) `org.springframework.web.context.request.RequestContextListener` `ServletRequestListener`를 등록해야 합니다. 이는 `WebApplicationInitializer` 인터페이스를 사용하여 프로그래밍 방식으로 수행할 수 있습니다. 또는 웹 애플리케이션의 `web.xml`파일에 다음 선언을 추가합니다

```xml
<web-app>
    ...
    <listener>
        <listener-class>
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```

또는 리스너 설정에 문제가 있는 경우 Spring의 `RequestContextFilter` 사용을 고려하십시오. 필터 매핑은 주변 웹 애플리케이션 구성에 따라 달라지므로 적절하게 변경해야 합니다. 다음 목록은 웹 애플리케이션의 필터 부분을 보여줍니다.

```xml
<web-app>
    ...
    <filter>
        <filter-name>requestContextFilter</filter-name>
        <filter-class>org.springframework.web.filter.RequestContextFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestContextFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ...
</web-app>
```

`DispatcherServlet`, `RequestContextListener` 및 `RequestContextFilter`는 모두 정확히 동일한 작업을 수행합니다. 즉, HTTP 요청 개체를 해당 요청을 서비스하는 `Thread`에 바인딩합니다. 이렇게 하면 요청 및 세션 범위의 Bean을 호출 체인에서 더 아래로 사용할 수 있습니다



### Request scope

Bean 정의에 대해 다음 XML 구성을 고려합니다.

```xml
<bean id="loginAction" class="com.something.LoginAction" scope="request"/>
```

Spring 컨테이너는 각각의 모든 HTTP 요청에 대해 `loginAction` bean definition를 사용하여 LoginAction 빈의 새 인스턴스를 생성합니다. 즉, `loginAction` 빈은 HTTP 요청 level에서 범위가 지정됩니다. 동일한 loginAction 빈 정의에서 생성된 다른 인스턴스는 이러한 상태 변화를 볼 수 없기 때문에 생성된 인스턴스의 내부 상태를 원하는 만큼 변경할 수 있습니다. 개별 요청에 따라 달라집니다. 요청 처리가 완료되면 요청 범위의 Bean이 삭제됩니다.



Annotation 기반 구성 요소 또는 Java 구성을 사용하는 경우 `@RequestScope` annotation을 사용하여 `request` 범위에 구성 요소를 할당 할 수 있습니다. 다음 예에서는 이를 수행하는 방법을 보여줍니다.

```java
@RequestScope
@Component
public class LoginAction {
    // ...
}
```



### Session Scope

bean definition에 대해 다음 XML 구성을 고려합니다.

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>
```

Spring 컨테이너는 단일 HTTP `Session`의 수명 동안 `userPreferences` bean definition를 사용하여 `UserPreferences` 빈의 새 인스턴스를 생성합니다. 즉, `userPreferences` 빈은 HTTP `Session` level에서 실제러으로 범위가 지정됩니다. request-scoped beans과 마찬가지로 생성된 인스턴스의 내부 상태를 원하는 만큼 변경할 수 있습니다.  개별 HTTP 세션에 고유하기 때문에 동일한 `userPreferences` bean definition에서 생성된 인스턴스를 사용하는 다른 HTTP `Session` 인스턴스는 이러한 상태 변경을 볼 수 없습니다. HTTP `Session`이 결국 폐기되면 해당 특정 HTTP `Session`으로 범위가 지정된 Bean도 폐기됩니다.



Annotation 기반 구성 요소 또는 Java 구성을 사용하는 경우 `@SessionScope` annotation을 사용하여 구성 요소를 session 범위에 할당할 수 있습니다.

```java
@SessionScope
@Component
public class UserPreferences {
    // ...
}
```

### **Application Scope**

bean definition에 대해 다음 XML 구성을 고려합니다.

```xml
<bean id="appPreferences" class="com.something.AppPreferences" scope="application"/>
```

Spring 컨테이너는 전체 웹 애플리케이션에 대해 한 번 `appPreferences` 빈 정의를 사용하여 `AppPreferences` 빈의 새 인스턴스를 생성합니다. 즉, `appPreferences` 빈은 `ServletContext` level에서 범위가 지정되고 일반 `ServletContext` 속성으로 저장됩니다. 이것은 Spring 싱글톤 빈과 어느 정도 비슷하지만 두 가지 중요한 점에서 다릅니다.&#x20;

* Spring `ApplicationContext`가 아닌 `ServletContext`당 싱글톤입니다(특정 웹 애플리케이션에 여러 개가 있을 수 있음)
* 실제로 exposed되어 `ServletContext` 속성으로 표시됩니다.

Annotation 기반 구성 요소 또는 Java 구성을 사용하는 경우 `@ApplicationScope` annotation을 사용하여 응용 프로그램 범위에 구성 요소를 할당할 수 있습니다. 다음 예에서는 이를 수행하는 방법을 보여줍니다.

```java
@ApplicationScope
@Component
public class AppPreferences {
    // ...
}
```

### **WebSocket Scope**

WebSocket 범위는 WebSocket 세션의 수명 주기와 연관되며 WebSocket 애플리케이션을 통한 STOMP에 적용됩니다. 자세한 내용은[ WebSocket 범위](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#websocket-stomp-websocket-scope)를 참조하세요. (예를 들어) HTTP 요청 범위 빈을 더 오래 지속되는 범위의 다른 빈에 주입하려는 경우 범위가 지정된 빈 대신 AOP 프록시를 의존하도록 선택할 수 있습니다. 즉, 범위가 지정된 객체와 동일한 공용 인터페이스를 exposes하지만 관련 범위(예: HTTP 요청)에서 실제 대상 객체를 검색할 수 있는 프록시 객체를 주입하고 실제 객체에 메서드 호출을 위임해야 합니다.



### **Scoped Beans as Dependencies**

Spring IoC 컨테이너는 객체(빈)의 인스턴스화뿐만 아니라 협력자(또는 의존성)의 연결도 관리합니다. (예를 들어) HTTP 요청 범위 빈을 더 오래 지속되는 범위의 다른 빈에 주입하려는 경우 범위가 지정된 빈 대신 AOP 프록시를 주입하도록 선택할 수 있습니다. 즉, 범위가 지정된 객체와 동일한 공용 인터페이스를 exposes하지만 관련 범위에서 실제 대상 객체를 검색할 수도 있는 프록시 객체(HTTP 요청과 같은)를 주입하거나 대리자 메서드를 사용하여 실제 책체를 호출하여야 한다.



{% hint style="info" %}
싱글톤으로 범위가 지정된 빈 사이에서 \<aop:scoped-proxy>를 사용할 수도 있습니다. 그리고 참조는 직렬화 가능한 중간 프록시를 통과하므로 역직렬화 시 대상 싱글톤 빈을 다시 얻을 수 있습니다.



범위 프로토타입의 bean에 대해 \<aop:scoped-proxy>를 선언할 때 공유 프록시에 대한 모든 메서드 호출은 호출이 전달되는 새 대상 인스턴스의 생성으로 이어집니다.



또한 범위가 지정된 프록시가 lifecycle-safe fashion에서 더 짧은 범위에서 Bean에 액세스하는 유일한 방법은 아닙니다. 인스턴스를 유지하거나 별도로 저장하지 않고 의존 point(즉, 생성자 또는 setter 인수 또는 autowired 필드)을 `ObjectFactory<MyTargetBean>`으로 선언하여 필요할 때마다 요청 시 현재 인스턴스를 검색하는 `getObject()` 호출을 허용할 수도 있습니다.



확장 변형으로 `getIfAvailable` 및 `getIfUnique`를 포함하여 몇 가지 추가 액세스 변형을 제공하는 `ObjectProvider<MyTargetBean>`을 선언할 수 있습니다.



이에 대한 JSR-330 변형 `Provider`라고 하며 모든 검색 시도에 대해 `Provider<MyTargetBean>` 선언 및 해당 `get()` 호출과 함께 사용됩니다. JSR-330 전체에 대한 자세한 내용은 [여기](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-standard-annotations)를 참조하십시오.
{% endhint %}



다음 예의 구성은 한 줄에 불과하지만 "why"와 "how"을 이해하는 것이 중요합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 프록시로 노출된 HTTP 세션 범위 빈 -->
    <bean id="userPreferences" class="com.something.UserPreferences" scope="session">
        <!-- 컨테이너가 주변 빈을 프록시하도록 지시합니다. -->
        <aop:scoped-proxy/> ❶
    </bean>

    <!-- 위의 bean에 대한 프록시와 함께 주입된 싱글톤 범위의 bean -->
    <bean id="userService" class="com.something.SimpleUserService">
        <!-- 프록시된 userPreferences 빈에 대한 참조 -->
        <property name="userPreferences" ref="userPreferences"/>
    </bean>
</beans>
```

❶ 프록시를 정의하는 줄입니다

이러한 프록시를 생성하려면 하위 `<aop:scoped-proxy/>` 요소를 범위 지정 bean definition에 삽입합니다([생성할 프록시 유형 선택](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-other-injection-proxies) 및 [XML 스키마 기반 구성](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#core.appendix.xsd-schemas) 참조). `request`, `session` 및 사용자 정의 범위 levels에서 범위가 지정된 Bean 정의에 `<aop:scoped-proxy/>` 요소가 필요한 이유는 무엇입니까? 다음 싱글톤 bean 정의를 고려하고 앞서 언급한 범위에 대해 정의해야 하는 것과 대조합니다. (다음 userPreferences 빈 정의는 불완전하다는 점에 유의합니다.)

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

이전 예제에서 싱글톤 빈(`userManager`)은 HTTP `Session` 범위 빈(`userPreferences`)에 대한 참조와 함께 주입됩니다. 여기서 보여주는  점은 `userManager` bean이 싱글톤이라는 것입니다. 컨테이너당 정확히 한 번 인스턴스화되며 해당 의존성(이 경우 하나만, userPreferences 빈)도 한 번만 주입됩니다. 이는 `userManager` bean이 정확히 동일한 `userPreferences` 객체(즉, 원래 삽입된 객체)에서만 작동함을 의미합니다.



수명이 짧은 범위의 Bean을 수명이 긴 범위의 Bean에 주입할 때(예: HTTP `Session` 범위의 협력 Bean을 의존성으로 싱글톤 Bean에 주입) 원하는 동작이 아닙니다. 오히려 단일 `userManager` 객체가 필요하고 HTTP `Session`의 수명 동안 HTTP `Session`에 특정한 `userPreferences` 객체가 필요합니다. 따라서 컨테이너는 범위 지정 메커니즘(HTTP 요청, `Session` 등)에서 실제 `UserPreferences` 객체를 가져올 수 있는 `UserPreferences` 클래스(이상적으로는 UserPreferences 인스턴스인 개체)와 정확히 동일한 공용 인터페이스를 expose하는 객체를 생성합니다. 컨테이너는 이 프록시 객체를 이 `UserPreferences` 참조가 프록시라는 것을 인식하지 못하는 `userManager` 빈에 주입합니다. 이 예에서 `UserManager` 인스턴스가 의존성 주입된 `UserPreferences` 객체에서 메서드를 호출하면 실제로는 프록시에서 메서드를 호출합니다. 그런 다음 프록시는 HTTP `Session`(이 경우)에서 실제 `UserPreferences` 객체를 가져오고 메서드 호출을 검색된 실제 `UserPreferences` 객체에 위임합니다.



따라서 다음 예제와 같이 `request-` 및 `session-scoped` 범위 빈을 협업 객체에 주입할 때 다음과 같은 (정확하고 완전한) 구성이 필요합니다.

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```



### Choosing the Type of Proxy to Create

기본적으로 Spring 컨테이너가 `<aop:scoped-proxy/>` 요소로 표시된 빈에 대한 프록시를 생성하면 CGLIB 기반 클래스 프록시가 생성됩니다.

{% hint style="info" %}
CGLIB 프록시는 공용 메서드 호출만 가로챕니다! 그러한 프록시에서 비공개 메서드를 호출하지 마십시오. 실제 범위가 지정된 대상 개체에 위임되지 않습니다.
{% endhint %}



또는 `<aop:scoped-proxy/>` 요소의 `proxy-target-class` 속성 값에 대해 `false`를 지정하여 이러한 범위가 지정된 Bean에 대한 표준 JDK 인터페이스 기반 프록시를 생성하도록 Spring 컨테이너를 구성할 수 있습니다. JDK 인터페이스 기반 프록시를 사용하기 때문에 이러한 프록시에 affect하기 위해 애플리케이션 클래스 경로에 추가 라이브러리가 필요하지 않습니다. 그러나 이는 범위가 지정된 Bean의 클래스가 적어도 하나의 인터페이스를 구현해야 하고 범위가 지정된 Bean이 주입되는 모든 협력자가 해당 인터페이스 중 하나를 통해 Bean을 참조해야 함을 의미합니다. 다음 예는 인터페이스 기반 프록시를 보여줍니다.

```xml
<!-- DefaultUserPreferences는 UserPreferences 인터페이스를 구현합니다. -->
<bean id="userPreferences" class="com.stuff.DefaultUserPreferences" scope="session">
    <aop:scoped-proxy proxy-target-class="false"/>
</bean>

<bean id="userManager" class="com.stuff.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

클래스 기반 또는 인터페이스 기반 프록시 선택에 대한 자세한 내용은 [프록시 메커니즘](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-proxying)을 참조하십시오.

## 5.5 Custom Scopes

Bean 범위 지정 메커니즘은 확장 가능합니다. 자신의 범위를 정의하거나 기존 범위를 재정의할 수도 있지만 후자는 나쁜 습관으로 간주되어 기본 제공 `singleton` 및 `prototype` 범위를 재정의할 수 없습니다.

### **Creating a Custom Scope**

사용자 지정 범위를 Spring 컨테이너에 통합하려면 이 섹션에서 설명하는 `org.springframework.beans.factory.config.Scope` 인터페이스를 구현해야 합니다. 자신의 범위를 구현하는 방법에 대한 아이디어는 Spring Framework 자체와 함께 제공되는 Scope 구현 및 구현해야 하는 메소드를 자세히 설명하는 [Scope](https://docs.spring.io/spring-framework/docs/6.0.8/javadoc-api/org/springframework/beans/factory/config/Scope.html) javadoc를 참조하세요.



`Scope` 인터페이스에는 범위에서 객체를 가져오고, 범위에서 제거하고, 소멸되도록 하는 네 가지 메서드가 있습니다.



예를 들어 session 범위 구현은 session 범위 빈을 반환합니다(존재하지 않는 경우 메서드는 나중에 참조할 수 있도록 세션에 바인딩한 후 빈의 새 인스턴스를 반환합니다). 다음 메서드는 기본 범위에서 객체를 반환합니다.

```java
Object get(String name, ObjectFactory<?> objectFactory)
```



예를 들어 세션 범위 구현은 기본 세션에서 세션 범위 빈을 제거합니다. 객체를 반환해야 하지만 지정된 이름의 객체를 찾을 수 없는 경우 `null`을 반환할 수 있습니다. 다음 메서드는 기본 범위에서 객체를 제거합니다.

```java
Object remove(String name)
```



다음 메서드는 범위가 소멸되거나 범위의 지정된 객체가 소멸될 때 범위가 호출해야 하는 콜백을 등록합니다.

```java
void registerDestructionCallback(String name, Runnable destructionCallback)
```

소멸 콜백에 대한 자세한 내용은 [javadoc](https://docs.spring.io/spring-framework/docs/6.0.8/javadoc-api/org/springframework/beans/factory/config/Scope.html#registerDestructionCallback) 또는 Spring 범위 구현을 참조하세요.



다음 메서드는 기본 범위에 대한 대화 식별자를 가져옵니다.

```java
String getConversationId()
```

이 식별자는 범위마다 다릅니다. 세션 범위 구현의 경우 이 식별자는 세션 식별자일 수 있습니다.

### **Using a Custom Scope**

하나 이상의 사용자 지정 `Scope` 구현을 작성하고 테스트한 후에는 Spring 컨테이너가 새 범위를 인식하도록 해야 합니다. 다음 방법은 Spring 컨테이너에 새 `Scope`를 등록하는 핵심 방법입니다.

```java
void registerScope(String scopeName, Scope scope);
```

이 메서드는 `ConfigurableBeanFactory`인터페이스에서 선언되며 Spring과 함께 제공되는 대부분의 구체적인 `ApplicationContext` 구현에서 `BeanFactory` 속성을 통해 사용할 수 있습니다.



`registerScope(..)` 메서드의 첫 번째 인수는 범위와 연결된 고유한 이름입니다. Spring 컨테이너 자체에서 그러한 이름의 예는 `singleton`과 `prototype`입니다. `registerScope(..)` 메서드에 대한 두 번째 인수는 등록하고 사용하려는 사용자 지정 범위 구현의 실제 인스턴스입니다.



사용자 지정 `Scope` 구현을 작성한 후 다음 예제와 같이 등록한다고 가정합니다.

{% hint style="info" %}
다음 예제에서는 Spring에 포함되어 있지만 기본적으로 등록되지 않은 `SimpleThreadScope`를 사용합니다. 사용자 지정 `Scope` 구현에 대한 지침은 동일합니다.
{% endhint %}



```java
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```

그런 다음 다음과 같이 사용자 지정 `Scope`의 범위 지정 규칙을 준수하는 bean definitions를 만들 수 있습니다.

```java
<bean id="..." class="..." scope="thread">
```

사용자 지정 `Scope` 구현을 사용하면 범위의 프로그래밍 방식 등록으로 제한되지 않습니다. 다음 예제와 같이 `CustomScopeConfigurer` 클래스를 사용하여 `Scope` 등록을 선언적으로 수행할 수도 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
        <property name="scopes">
            <map>
                <entry key="thread">
                    <bean class="org.springframework.context.support.SimpleThreadScope"/>
                </entry>
            </map>
        </property>
    </bean>

    <bean id="thing2" class="x.y.Thing2" scope="thread">
        <property name="name" value="Rick"/>
        <aop:scoped-proxy/>
    </bean>

    <bean id="thing1" class="x.y.Thing1">
        <property name="thing2" ref="thing2"/>
    </bean>

</beans>
```

{% hint style="info" %}
`FactoryBean` 구현에 대한 `<bean>`선언 내에 `<aop:scoped-proxy>`를 배치하면 `getObject()`에서 반환된 객체가 아니라 범위가 지정되는 팩토리 빈 자체입니다.
{% endhint %}

##

## Trying.....

