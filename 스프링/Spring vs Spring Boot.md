## Spring 과 Spring Boot 의 차이점에 대해서 설명해주세요.

Spring Boot 는 Spring 을 보다 편리하게 이용할 수 있도록 도움을 주는 프로젝트입니다.

Spring Boot 는 Spring 에서 복잡하게 했던 설정들을 Auto Configuration 을 통해 자동화해주는 기능을 제공하고, Dependency 관리를 통해서 해당 Dependency 의 버전을 직접 명시하는 것이 아니라 사용하는 Spring Boot 의 버전에 호환되는 버전을 자동으로 가져와주는 기능을 제공합니다. 또한 내장 톰캣을 사용하기 때문에 톰캣이 설치되어 있지 않은 환경에서도 .jar 파일만 가지고 서버를 실행시킬 수 있다는 장점이 있고, 뿐만 아니라 서버의 실행속도 또한 더 빠르게 개선되었습니다.

<br>

## Spring Boot 에서 Dependency 를 어떻게 관리하나요?

우선 Spring Boot 에서는 기능별로 Starter Dependency 를 제공해 해당 기능을 이용하는데 필요한 Dependency 를 묶어서 제공합니다. 예를들어 Spring AOP 를 이용하고 싶다면 spring-boot-starter-aop Dependency 만 추가하면 필요한 모든 Dependency 가 추가됩니다.

또한 사용하려는 Dependency 의 버전을 일일히 입력하지 않아도 되고 Spring Boot 가 자동으로 Spring Boot 버전에 호환성이 맞는 버전으로 주입해줍니다. 이러한 방식이 가능한 이유는 Spring Boot Starter Parent 를 부모 pom.xml 으로 설정해 해당 버전에 대해서 호환되는 Dependency 의 버전정보를 미리 매핑시켜놨기 때문입니다.

이렇게 버전정보를 입력하지 않음으로써 특정 라이브러리를 도입할 때 Spring Boot 와 호환성이 맞는 버전을 고민하지 않아도 되고, Spring Boot 의 버전을 업그레이드하거나 다운그레이드할 때 의존성이 맞는 버전으로 수정하는 번거로움이 없게됩니다.