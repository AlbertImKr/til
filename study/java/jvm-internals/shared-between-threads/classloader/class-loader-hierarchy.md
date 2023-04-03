# Class Loader Hierarchy

VM에는 역할이 다른 여러 클래스 로더가 있습니다. 각 클래스 로더는 최상위 클래스 로더인 **bootstrap classloader**를 제외하고 상위 클래스 로더(로드한)에 위임합니다.

<figure><img src="../../../../../.gitbook/assets/class_loader_hierarchy.png" alt=""><figcaption></figcaption></figure>

## Bootstrp ClassLoader

Bootstrap Classloader는 JVM이 로드될 때 매우 일찍 인스턴스화되기 때문에 일반적으로 기본 코드로 구현됩니다. Bootstrap Classloader는 (예를 들어 rt.jar를 포함) 기본 Java API를 로드하는 역할을 합니다. 신뢰 수준이 더 높은 boot 클래스 경로에 있는 클래스만 로드합니다. 결과적으로 일반 클래스에 대해 수행되는 많은 유효성 검사를 건너뜁니다.

## Extension ClassLoader

Extension Classloader는 보안 확장 기능과 같은 표준 Java 확장 API에서 클래스를 로드합니다.

## System Classloader

System Classloader는 클래스 경로에서 애플리케이션 클래스를 로드하는 기본 애플리케이션 클래스 로더입니다.



## User Defined Classloaders

사용자 정의 클래스 로더는 애플리케이션 클래스를 로드하는 데 사용할 수 있습니다. 사용자 정의 클래스 로더는 클래스의 런타임 재로딩 또는 일반적으로 Tomcat과 같은 웹 서버에 필요한 로드된 클래스의 다른 그룹 간의 분리를 포함하여 여러 가지 특별한 이유로 사용됩니다.
