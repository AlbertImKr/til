---
description: Version 6.0.7
---

# IoCContainer

## 1. Spring Ioc 컨테이너 및 Bean 소개



이 장에서는 Inversion of Controller(IoC) 원칙의 Spring Framework 구현을 다룹니다. IoC는 의존성 주입(DI)이라고 합니다. 객체가 생성자 인수, 팩토리 메서드에 대한 인수 또는 생성된 객체 인스턴스나 팩토리 메서드에서 반환된 객체 인스턴스에 설정된 속성을 통해서만 객체의 의존성(즉, 그들과 함께 일하는 다른 객체)을 정의하는 포로세스입니다. 그런 다음 컨테이너는 빈을 생성할 때 이러한 이존성을 주입합니다. 이 프로세스는 근본적으로 서비스 로케이터 패턴과 같은 메커니즘 또는 클래스의 직접 구성을 통하여 의존성의 인스턴스화 또는 위치를 제어하는 빈 자체의 역전(따라서 이름, Inversion of Control)입니다.



<mark style="background-color:yellow;">**org.springframeworkd.beans**</mark> 및 <mark style="background-color:yellow;">**org.springframework.context**</mark> 패키지는 Spring Framework의 IoC 컨테이너의 기반입니다. [BeanFactory](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/beans/factory/BeanFactory.html) 인터페이스는 모든 유형의 객체를 관리할 수 있는 고급 구성 메커니즘을 제공합니다. [ApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/ApplicationContext.html)는 <mark style="background-color:yellow;">**BeanFactory**</mark>의 하위 인터페이스입니다.

ApplicationContext 다음을 추가한다.

* Spring의 AOP 기능과 더 쉽게 통합
* 메시지 리소스 처리(국제화에 사용)
* 이벤트 발행
* 애플리케이션 계층 특정 Context 예를 들어 웹 애플리케이션에서 사용하는 <mark style="background-color:yellow;">**webApplicationContext**</mark>

즉, <mark style="background-color:yellow;">**BeanFactory**</mark>는 구성 프레임워크와 기본 기능을 제공하고 <mark style="background-color:yellow;">**ApplicationContext**</mark> 더 많은 enterprise-specific 기능을 추가합니다. <mark style="background-color:yellow;">**ApplicationContext**</mark>는 <mark style="background-color:yellow;">**BeanFactory**</mark>의 완성된 상위집합이며 이 장에서는 오직 Spring의 IoC컨테이너를 설명하기 위해 사용됩니다. <mark style="background-color:yellow;">**BeanFactory**</mark> 대신 <mark style="background-color:yellow;">**ApplicationContext**</mark> 사용에 대한 자세한 내용은 [BeanFactory API](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-beanfactory) 섹션을 참조하십시오.



Spring에서 애플리케이션의 중추를 형성하고 Spring IoC 컨테이너에 의해 관리되는 객체를 빈이라고 합니다. Bean은 Spring IoC 컨테이너에 의해 인스턴스화, 조립 및 관리되는 객체입니다. 그렇지 않으면 빈은 애플리케이션의 많은 객체 중 하나일 뿐입니다. Bean과 이들 간의 의존성은 컨테이너에서 사용하는 Configuration Metadata에 반영됩니다.



## 2. Container OverView



<mark style="background-color:yellow;">**org.springframework.context.ApplicationContext**</mark> 인터페이스는 Spring IoC 컨테이너를 표현하며 beans 인스턴스화, 구성 및 조립을 담당합니다. 컨테이너는 Configuration Metadata를 읽어 객체를 인스턴스화, 구성 및 어셈블리한다. Configuration Metadata는 XML , Java annotations 또는 Java코드로 표시한다. application을 구성하는 객체와 객체 간의 풍부한 상호 의존성을 표현할 수 있습니다.\


<mark style="background-color:yellow;">**ApplicationContext**</mark> 인터페이스의 여러 구현은 Spring과 함께 제공됩니다. 독립 실행형 Applications에서는 일반적으로 [ClassPathXmlApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html) 또는 [FileSystemXmlApplicationContext](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html)의 인스턴스를 만든다. XML은 Configuration Metadata를 정의하는 전통적인 형식이지만 소량의 XML 구성을 통하여 컨테이너에 Java annotations 또는 코드 같은 추가된 Metadata 형식을 사용할 수 있도록 할 수 있다.



대부분의 애플리케이션 시나리오에서 하나 이상의 Spring IoC 컨테이너 인스턴스를 인스턴스화하는 데 명시적인 사용자 코드가 필요하지 않습니다. 예를 들어, 웹 애플리케이션 시나리오에서 일반적으로 애플리케이션의 <mark style="background-color:yellow;">**web.xml**</mark> 파일에 있는 간단한 8줄 정도의 상용구 웹 설명자의 XML로도 충분합니다([Convenient ApplicationContext Instantiation for Web Applications](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-create) 참조). [Spring Tools for Eclipse](https://spring.io/tools)(Eclispse 기반 개발 환경)을 사용하면 몇 번의 마우스 클릭이나 키 입력으로 이 상용구 구성을 쉽게 만들 수 있습니다.



다음 다이어그램은 Spring이 작동하는 방식에 대한 높은 수준의 뷰를 보여줍니다. 애플리케이션 클래스는 Configuration Metadata와 결합되어 <mark style="background-color:yellow;">**ApplicationContext**</mark>를 생성하고 초기화된 후 완전히 구성되고 실행 가능한 시스템 또는 애플리케이션을 만든다.

<figure><img src="https://blog.kakaocdn.net/dn/cQmWQz/btr5R1y8QQ7/QKNik0K3USF3e95uzrVqNk/img.png" alt=""><figcaption></figcaption></figure>

## **2.1 Configuration Metadata**

****

앞의 다이어그램에서 볼 수 있듯이 Spring IoC 컨테이너는 Configuration metadata 형식을 사용합니다. 이 configuration metadata는 애플리케이션 개발자로서 애플리케이션에서 객체를 인스턴스화, 구성 및 어셈블 하도록 Spring 컨테이너에 지시하는 방법을 표시합니다.



Configuration metadata는 전통적으로 간단하고 직관적인 XML 형식으로 제공되며 이 장의 대부분은 Spring IoC 컨테이너의 주요 개념과 기능을 전달하기 위해 사용됩니다.



{% hint style="info" %}
XML 기반 metadata는 유일하게 허용되는 configuration metadata 형식이 아닙니다. Spring IoC 컨테이너 자체는 이 configuration metadata가 실제로 작성한 형식에서 완전히 분리됩니다. 요즘 많은 개발자들이 Spring 애플리케이션을 위해 [Java-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java)을 선택합니다.
{% endhint %}



Spring 컨테이너에서 다른 형식의 metadata를 사용하는 방법에 대한 자세한 내용은 다을 참조하세요.

* [Annotation-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config): annotation-based configuration metadata를 사용하여 beans을 정의합니다.
* [Java-based configuration](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java): XML 파일이 아닌 Java를 사용하여 애플리케이션 클래스 외부에서 beans를 정의합니다. 이러한 기능을 사용하려면 [@Configuration](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Configuration.html), [@Bean](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Bean.html), [@Import](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/Import.html), and [@DependsOn annotations](https://docs.spring.io/spring-framework/docs/6.0.7/javadoc-api/org/springframework/context/annotation/DependsOn.html)을 참조하십시오.



Spring configuraion은 컨테이너가 관리해야 하는 적어도 하나 일반적으로 둘 이상의 bean definition로 구성됩니다. XML-based configuration metadata는 이러한 빈을 최상위 요소 <mark style="background-color:yellow;">**\<beans/>**</mark>내의 <mark style="background-color:yellow;">**\<bean/>**</mark>요소로 구성됩니다. Java configuration은 일반적으로 <mark style="background-color:yellow;">**@Configuration**</mark> 클래스 내에서 <mark style="background-color:yellow;">**@Bean**</mark> -annotated 메서드를 사용합니다.



이러한 bean definitions는 애플리케이션을 구성하는 실제 객체에 해당합니다. 일반적으로 서비스 계층 객체, 저장소 또는 DAO(데이터 액세스 개체)와 같은 지속성 계층 객체, 웹 컨트롤러와 같은 프레젠테이션 객체, JPA <mark style="background-color:yellow;">**EntityManagerFactory**</mark>와 같은 인프라 객체, JMS 대기열 등을 정의합니다. 일반적으로 도메인 객체를 만들고 로드하는 것은 repositories와 비즈니스 로직의 책임이기 때문에 컨테이너에 세부적인 도메인 객체를 구성하지 않습니다.



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

****

<mark style="background-color:yellow;">**ApplicationContext**</mark> 생성자에 제공되는 위치 경로는 컨테이너가 로컬 파일 시스템, Java <mark style="background-color:yellow;">**CLASSPATH**</mark> 등과 같은 다양한 외부 리소스에서 configuration metadata를 로드할 수 있도록 하는 리소스 문자열입니다.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

{% hint style="info" %}
Spring의 IoC 컨테이너에 대해 배운 후 URI 구문에 정의된 위치에서 InputStream을 읽기 위한 편리한 메커니즘을 제공하는 Spring의 Resource 추상화([Resources](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#resources) 참조)에 대해 더 알고 싶을 수 있습니다. 특히 Resource 경로는 애플리케이 contexts(\
[Application Contexts and Resource paths](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#resources-app-ctx) 참조)을 구성하는 데 사용됩니다.
{% endhint %}



다음 예는 서비스 계층 객체(<mark style="background-color:yellow;">**services.xml**</mark>) configuration 파일을 보여줍니다.

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

다음 예는 데이터 액세스 객체 <mark style="background-color:yellow;">**daos.xml**</mark> 파일을 보여줍니다.

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

앞의 예에서 서비스 계층은 <mark style="background-color:yellow;">**PetStoreServiceImpl**</mark> 클래스와 <mark style="background-color:yellow;">**JpaAccountDao**</mark> 및 <mark style="background-color:yellow;">**JpaItemDao**</mark> 유형의 두 데이터 액세스 객체(JPA 객체-관계형 매핑 표준 기반)로 구성됩니다. <mark style="background-color:yellow;">**property name**</mark> 요소는 JavaBean 속성의 이름을 참조하고 <mark style="background-color:yellow;">**ref**</mark> 요소는 다른 bean정의의 이름을 참조합니다. <mark style="background-color:yellow;">**id**</mark>와 <mark style="background-color:yellow;">**ref**</mark> 요소 간의 이러한 연결은 공통 작업 개체 간의 의족성을 나타냅니다. 객체의 의존성 구성에 대한 자세한 내용은 [Dependencies을](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-dependencies) 참조하십시오.



### **XML-based Configuration Metadata 구성**

****

Bean definitions가 여러 XML 파일에 걸쳐 있는 것이 유용할 수 있습니다. 각 XML 구성 파일은 종종 아키텍처의 논리적 계층 또는 모듈을 표현합니다.



애플리케이션 context 생성자를 사용하여 이러한 모든 XML 조각에서 bean definitions을 로드할 수 있습니다. 이 생성자는 [이전 세션](ioccontainer.md#2.2)에 표시된 것처럼 여러 <mark style="background-color:yellow;">**Resource**</mark> 위치를 사용합니다. 또는 하나 이상의 <mark style="background-color:yellow;">**\<import/>**</mark> 요소를 사용하여 다른 파일에서 bean definitions를 정의합니다. 다음 예에서는 이를 수행하는 방법을 보여줍니다.

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

예제에서 외부 bean definitions는 <mark style="background-color:yellow;">**services.xml**</mark>, <mark style="background-color:yellow;">**messageSource.xml**</mark> 및 <mark style="background-color:yellow;">**themeSource.xml**</mark>의 세 파일에서 로드됩니다. 모든 위치 경로는 importing을 수행하는 definition파일과 관계있다. 즉 <mark style="background-color:yellow;">**services.xml**</mark>은 importing을 수행하는 파일과 동일한 디렉터리 또는 클래스 위치에 있어야 하며 <mark style="background-color:yellow;">**messageSource.xml**</mark> 및 <mark style="background-color:yellow;">**themeSource.xml**</mark>은 importing 파일 위치 아래의 <mark style="background-color:yellow;">**resources**</mark> 위치 안에 있어야 한다. 보시다시피 선행 슬래시는 무시됩니다. 그러나 이러한 경로가 상대적인 경우 슬래시를 전형 사용하지 않는 것이 더 좋습니다. 최상위 레벨 <mark style="background-color:yellow;">**\<beans/>**</mark> 요소를 포함하여 imported 한 파일의 내용은 Spring Schema에 따라 유효한 XML bean definitions여야 한다.



{% hint style="info" %}
상대 "../" 경로 사용하여 상위 디렉터리의 파일을 참조하는 것은 가능하지만 권장하지 않습니다. 이렇게 하면 현재 응용 프로그램 외부에 있는 파일에 대한 의존성이 생성됩니다. 특히 런타임 확인 프로세서를 통하여 "가장 가까운" 클래스 경로 root를 선택 후 다음 상위 디렉터리를 찾는 <mark style="background-color:yellow;">**classpath:**</mark> URLs(예: <mark style="background-color:yellow;">**classpath:../services.xml**</mark>)의 참조를 권장하지 않습니다. Classpath configuration 변경으로 인한 다른 잘못된 디렉터리가 선택될 수 있습니다.\
\
상대 경로 대신 정규회 된 리소스 위치를 사용할 수 있습니다. 예: <mark style="background-color:yellow;">**file:C:/config/services.xml**</mark> or <mark style="background-color:yellow;">**classpath:/config/services.xml**</mark>. 그러나 애플리케이션의 configuration을 특정 절대 위치에 연결하고 있다는 점에 유의하십시오.  일반적으로 이러한 절대 위치에 대한 간접 참조를 유지하는 것이 좋습니다.  예를 들어 런타임 시 JVM 시스템 속성으로  확인하는  "${... }" placeholders를 사용한다.
{% endhint %}

namespace 자체는 import 지시 기능을 제공합니다.  일반 bean definitions을 넘어서는 추가 기능은 Spring에서 제공하는 XML namespaces(예: <mark style="background-color:yellow;">**context**</mark> 및 <mark style="background-color:yellow;">**util**</mark> namespaces)에서 사용할 수 있습니다.

### **The Groovy Bean Definition DSL**

****

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

이 configuration 스타일은 대체로 XML bean definitions와 동일하며 Spring의 XML configuration namespaces도 지원합니다.또한 <mark style="background-color:yellow;">**importBeans**</mark> 명령을 통해 XML bean definition 파일을 imporing할 수 있습니다.



## **2.3 컨테이너 사용**

****

<mark style="background-color:yellow;">**ApplicationContext**</mark>는 서로 다른 beans과 그 의존성의 레지스트리를 잘 유지할 수 있는 고급 factory 인터페이스입니다. <mark style="background-color:yellow;">**T getBean(String name, Class \<T> requiredType)**</mark> 메서드를 사용하여 beans의 인스턴스룰 검색할 수 있습니다.\


<mark style="background-color:yellow;">**ApplicationContext**</mark>를 사용하면 다음 예제와 같이 bean definitions를 읽고 액세스 할 수 있습니다.

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

가장 유연한 변형은 reader delegates와 결합된 <mark style="background-color:yellow;">**GenericApplicationContext**</mark>입니다. 예를 들어 다음 예제와 같이 XML 파일용 <mark style="background-color:yellow;">**XmlBeanDefinitionReader**</mark>를 사용합니다.

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

다음 예제와 같이 Groovy 파일용 <mark style="background-color:yellow;">**GroovyBeanDefinitionReader**</mark>를 사용할 수도 있습니다.

```java
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

동일한 <mark style="background-color:yellow;">**ApplicationContext**</mark>에서 이러한 reader delegates를 믹스 및 매치하여 다양한 구성 소스에서 bean definitions을 읽어 올 수 있습니다.



그런 다음 <mark style="background-color:yellow;">**getBean**</mark>을 사용하여 beans의 인스턴스를 검색할 수 있습니다. <mark style="background-color:yellow;">**ApplicationContext**</mark> 인터페이스는 bean을 검색하기 위한 몇가지 다른 메서드가 있지만 이상적으로 애플리케이션 코드에서 이를 사용해서는 안 됩니다. 실제로 애플이케이션 코드에는 <mark style="background-color:yellow;">**getBean()**</mark> 메서드에 대한 호출이 전혀 없어야 하며 따라서 Spring API에 대한 의존성이 전혀 없어야 한다.  예를 들어 Spring의 웹 프레임워크의 통합은 컨트롤러 및 JSF-managed beans과 같은 다양한 웹 프레임워크 구성 요소에 대한 의존성 주입을 제공하여 metadata(예: autowiring annotation)를 통해 특정 bean에 대한 의존성을 선언할 수 있습니다.



## 3. Bean Overview



Spring IoC 컨테이너는 하나 이상의 빈을 관리합니다. 이러한 beans는 컨테이너에 제공하는 configuration metadata(예: XML <mark style="background-color:yellow;">**\<bean/>**</mark> definitions의 형식)로 생성됩니다.



컨테이너 자체 내에서 이러한 beans definitions는 <mark style="background-color:yellow;">**BeanDefinition**</mark> objects로 표현되고 아래와 같은(기타 정보 중) metadata가 포함된다.

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
특정 bean을 생성하는 방법에 대한 정보를 포함하는 bean definitions 외에도 <mark style="background-color:yellow;">**ApplicationContext**</mark> 구현은 (사용자에 의해) 컨테이너 외부에서 생성된 기존 객체의 등록을 허용합니다. 이는 <mark style="background-color:yellow;">**DefaultListableBeanFactory**</mark> 구현을 반환하는 <mark style="background-color:yellow;">**getBeanFactory()**</mark> 메서드를 통해 ApplicationContext의 <mark style="background-color:yellow;">**BeanFactory**</mark>에 accessing 하여 실행합니다. <mark style="background-color:yellow;">**DefaultListableBeanFactory**</mark>는 <mark style="background-color:yellow;">**registerSingleton(..)**</mark>** ** 및 <mark style="background-color:yellow;">**registerBeanDefinition(..)**</mark> 메서드를 통해 이 등록을 지원합니다.&#x20;



{% hint style="info" %}
Bean metadata와 수동으로 제공된 싱글톤 인스턴스는 컨테이너가 autowiring 및 내부 검사 단계 과정에서 제대로 추론할 수 있도록 가능한 한 빨리 등록해야 합니다. 기존 metadata 및 기존 싱글톤 인스턴스를 overriding 하는 것은 지원되지만 런타임 시 새로운 beans 등록은 (동시에 라이브로 factory를 accsss) 공식적으로 지원되지 않으며 동시 access 예외 및 빈 컨테이너의 일관성 없는 상태를 도래될 수 있습니다.
{% endhint %}



## 3.1 Naming Beans



모든 빈에는 하나 이상의 식별자가 있습니다. 이러한 식별자는 bean을 호스팅 하는 컨테이너 내에서 고유해야 합니다. bean에는 일반적으로 식별자가 하나만 있습니다. 그러나  둘 이상이 필요한 경우 추가 항목은 별칭으로 간주될 수 있습니다.&#x20;



XML-based configuration medata에서 <mark style="background-color:yellow;">**id**</mark> 속성, <mark style="background-color:yellow;">**name**</mark> 속성 또는 둘 다 사용하여 bean 식별자를 지정합니다.  <mark style="background-color:yellow;">**id**</mark> 속성을 사용하면 정확히 하나의 <mark style="background-color:yellow;">**id**</mark>를 지정할 수 있습니다. 일반적으로 이러한 영숫자 ('myBean', 'someService' 등)이지만 특수 문자도 포함할 수 있습니다. bean에 대한 다른 별칭을 도입하려는 경우 <mark style="background-color:yellow;">**name**</mark> 속성에 쉼표(<mark style="background-color:yellow;">**,**</mark>), 세미콜론(<mark style="background-color:yellow;">**;**</mark>) 또는 공백으로 구분하여 지정할 수도 있습니다. id 속성은 <mark style="background-color:yellow;">**xsd:string**</mark> 타입으로 지정되지만 bean <mark style="background-color:yellow;">**id**</mark> 고유성은 XML 파서가 아니라 컨테이너에 의해 강제됩니다.



Bean의 <mark style="background-color:yellow;">**name**</mark>이나 <mark style="background-color:yellow;">**id**</mark>를 제공할 필요가 없습니다. <mark style="background-color:yellow;">**name**</mark>이나 <mark style="background-color:yellow;">**id**</mark> 를 명시적으로 제공하지 않으면 컨테이너는 해당 빈에 대해 고유한 이름을 생성합니다. 그러나 <mark style="background-color:yellow;">**ref**</mark> 요소 또는 Service Locator 스타일로 조회를 사용하여 해당 bean을 이름으로 참조하려면 이름을 반드시 제공해야 합니다. 이름을 제공하지 않아도 되는 원인은 [inner beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-inner-beans) 및 [autowiring collaborators](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire) 과 관련이 있습니다.



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



이 경우 <mark style="background-color:yellow;">**forName**</mark>이라는 빈(동일한 컨테이너에 있음)은 alias definition를 사용한 후 <mark style="background-color:yellow;">**toName**</mark>으로 참조될 수도 있습니다.



예를 들어 하위 시스템 A의 configuration metadata는 <mark style="background-color:yellow;">**subsystemA-dataSource**</mark>라는 이름으로 DataSource를 참조할 수 있습니다. 서브시스템 B에 configuration metadata는 <mark style="background-color:yellow;">**subsystemB-dataSource**</mark>라는 이름으로 DataSource를 참조할 수 있습니다. 이 두 하위 시스템을 모두 사용하는 main 애플리케이션을 구성할 때 main 애플리케이션은 <mark style="background-color:yellow;">**myApp-dataSource**</mark>라는 이름으로 DataSource를 참조합니다.  세 개의 이름이 모두 동일한 개체를 참조하도록 하려면 다음 alias definitions를 configuration metadata에 추가할 수 있습니다.

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



XML-based configuration metadata를 사용하는 경우 <mark style="background-color:yellow;">**\<bean/>**</mark> 요소의 <mark style="background-color:yellow;">**class**</mark> 속성에 인스턴스화할 객체의 타입(또는 클래스)을 지정합니다. 이 클래스 속성 (내부적으로 BeanDefinition 인스턴스의 Class 속성)은 일반적으로 필수입니다. (예외는 [Instantiation by Using an Instancee Factory Method](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-class-instance-factory-method)와 [Bean Definition Inheritance](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-child-bean-definitions) 참조하십시오). 두 가지 방법 중 하나로 Class 속성을 사용할 수 있습니다.

* 일반적으로 컨테이너 자체가 직접 반사적으로 생성자를 호출하여 bean을 직접 생성하는 경우에 생성될 bean 클래스를 지정하기 위해 <mark style="background-color:yellow;">**new**</mark> 연산자를 사용하는 Java 코드와 다소 동일합니다.
* 덜 일반적인 경우 객체를 생성하기 위해 호출되는 <mark style="background-color:yellow;">**static**</mark> 팩토리 메서드를 포함하는 실제 클래스를 지정하기 위해 컨테이너가 bean을 생성하기 위해 클래스에서 <mark style="background-color:yellow;">**static**</mark> 팩토리 메서드를 호출한다. <mark style="background-color:yellow;">**static**</mark> 팩토리 메서드 호출에서 반환된 객체 타입은 동일한 클래스이거나 완전히 다른 클래스일 수 있습니다.



{% hint style="info" %}
**중첩된 클래스 이름**

중첩된 클래스에 bean definition을 구성하려는 경우 중첩 클래스의 이진 이름 또는 소스 이름을 사용할 수 있습니다.



예를 들어 com.example 패키지에 <mark style="background-color:yellow;">**SomeThing**</mark>이라는 클래스가 있고 이 <mark style="background-color:yellow;">**SomeThing**</mark> 클래스에 <mark style="background-color:yellow;">**OtherThing**</mark>이라는 중첩 <mark style="background-color:yellow;">**static**</mark> 클래스가 있는 경우 달러 기호(<mark style="background-color:yellow;">**$**</mark>) 또는 점(<mark style="background-color:yellow;">**.**</mark>)으로 구분할 수 있습니다. 따라서 bean definition에서 클래스 속성의 값은 <mark style="background-color:yellow;">**com.example.SomeThing$OtherThing**</mark> 또는 <mark style="background-color:yellow;">**com.example.SomeThing.OtherThing**</mark>이 됩니다.
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



<mark style="background-color:yellow;">**static**</mark> 팩토리 메서드로 생성하는 bean을 정의할 때 클래스 속성을 사용하여 <mark style="background-color:yellow;">**static**</mark> 메소드를 포함하는 클래스를 지정하고 <mark style="background-color:yellow;">**factory-method**</mark>라는 속성을 사용하여 팩토리 메서드 자체의 이름을 지정합니다. (나중에 설명하는 선택적 인수를 사용하여) 이 메서드를 호출하고 활성 객체를 반환할 수 있어야 합니다. 이후에 이 객체는 생성자를 통해 생성된 것처럼 처리됩니다. 이러한 bean definition의 한 가지 용도는 레거시 코드에서 <mark style="background-color:yellow;">**static**</mark> 팩토리를 호출하는 것입니다.



다음 bean definition는 팩토리 메서드를 호출하여 빈이 생성되도록 지정합니다. definition은 반환된 객체의 타입(클래스)을 지정하지 않고 오히려 팩토리 메서드를 포함하는 클래스를 지정합니다. 이 예제에서 <mark style="background-color:yellow;">**createInstance()**</mark> 메서드는 <mark style="background-color:yellow;">**static**</mark> 메서드여야 한다. 다음 예제에서는 팩토리 메서드를 지정하는 방법을 보여줍니다.

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

[정적 팩토리 메서드를 통한 인스턴스화](ioccontainer.md#undefined-1)와 유사하게 인스턴스 팩토리 메서드를 사용한 인스턴스화는 컨테이너에서 기존 빈의 비정적 메서드를 호출하여 새 bean을 생성합니다. 이 메커니즘을 사용하려면 <mark style="background-color:yellow;">**class**</mark> 속성을 비워두고 <mark style="background-color:yellow;">**factory-bean**</mark> 속성에서 객체를 생성하기 위해 호출할 인스턴스 메서드를 포함하는 현재(또는 부모 또는 조상) 컨테이너의 빈 이름을 지정합니다. <mark style="background-color:yellow;">**fatory-method**</mark> 속성으로 팩토리 메서드 자체의 이름을 설정합니다.&#x20;

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

특정 빈의 런타임 타입은 결정하기 쉽지 않습니다. bean metadata definition에 지정된 클래스는 초기 클래스 참조일 뿐이며 선언된 팩토리 메서드와 결합되거나 또는 bean의 다른 런타임 타입으로 이어질 수 있는 <mark style="background-color:yellow;">**FactoryBean**</mark> 클래스이거나 인스턴스 레벨 팩토리 메서드(대신 지정된 factory-bean 이름을 통해 해결됩니다.)의 경우 전형 설정되지 않는다.  또한 AOP 프록싱은 대상 빈의 실제 유형(구현된 인터페이스만)을 제한적으로 노출하는 interface-based 프록시로 bean 인스턴스를 래핑 할 수 있습니다.



특정 bean의 실제 런타임 타입을 찾는 권장 방법은 지정된 bean 이름에 대한 <mark style="background-color:yellow;">**BeanFactory.getType**</mark> 호출입니다. 이것은 위의 모든 경우를 고려하고 BeanFactory.getBean 호출이 동일한 빈 이름에 대해 반환할 객체 타입을 반환합니다.



**Comming Soon**

