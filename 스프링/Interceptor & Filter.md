## Interceptor 와 Filter 의 공통점에 대해서 설명해주세요.

Interceptor 와 Filter 의 공통점으로는 클라이언트가 특정 URI 로 HTTP 요청을 보내고 HTTP 응답을 받는 과정을 가로채 추가적인 작업을 수행한다는 것입니다.

<br>

## 그러면 Interceptor 와 Filter 의 차이점에는 어떠한 것들이 있을까요?

우선 가장 큰 차이점은 가로채는 시점이라고 할 수 있을 것 같습니다.

Filter 는 Spring Context 밖에서 즉, Servlet Dispatcher 에 도달하기 전에 HTTP 요청을 가로채는 반면, Interceptor 는 Spring Context 내부에서 즉, Servlet Dispatcher 를 지나 Controller 의 Handler 로 가기 전에 가로챈다는 차이점이 있습니다.

또한 Filter 는 Interceptor 와 다르게 HTTP Request 나 Response 의 헤더 및 바디를 수정할 수 있고, Redirect 와 같은 방법을 이용해 해당 패킷의 흐름을 바꿀수도 있습니다.

반면에 Interceptor 는 Spring Context 내부에 존재하기 때문에 Spring IoC 컨테이너에 포함되어 있는 Bean 에 접근할 수 있다는 차이점이 있습니다. 이에 따라서 Filter 는 예외가 발생했을 때 직접 예외에 대한 처리를 해줘야 하지만 Interceptor 는 ControllerAdvice 를 이용한 Exception Handler 를 이용해 예외를 처리할 수 있다는 장점이 있습니다.

<br>

## Interceptor 와 Filter 를 각각 언제 사용할 수 있나요?

우선 Filter 는 Spring 과 무관한 작업을 전역적으로 처리해야 하는 상황에 사용할 수 있을 것 같습니다. 예를 들어보면 XSS 를 막기 위한 로직과 같은 보안관련 작업이 있을 것 같습니다. 또 HTTP Request 와 Response 를 수정할 수 있다는 점에서 특정 쿠키 추가 및 인코딩과 같은 작업을 Filter 를 통해서 할 수 있을 것 같고 마지막으로 특정 조건이 맞지 않는 경우 특정 경로로 Redirect 하는 작업또한 Filter 를 통해서 할 수 있을 것 같습니다.

Interceptor 는 Spring Context 에 등록되어 동작하기 때문에 Spring IoC 컨테이너에 포함된 Bean 을 이용해야하는 Spring 관련 작업을 전역적으로 처리하기 위해 사용할 수 있을 것 같습니다. 예를 들면 Spring Security 를 이용한 사용자 인증, 인가작업이 있을 것 같습니다.

<br>

## Interceptor 와 Filter 에 대해서 이야기를 했는데, 이게 Spring AOP 와 비슷한 역할을 수행하는 것 같은데 혹시 어떤 차이점이 있나요?

AOP 와의 첫 번째 차이점은 Interceptor 와 Filter 는 클라이언트의 URI 를 기준으로만 대상을 지정할 수 있지만 AOP 는 AspectJ 표현식을 이용해 어노테이션, 함수 형식등 다양한 형태로 대상을 지정할 수 있습니다.

그리고 Interceptor 와 Filter 는 HTTP Request 단위로만 지정이 가능하지만 AOP 는 메소드 단위로도 지정이 가능하다는 차이점이 있습니다.

<br>

## Filter 와 Interceptor 그리고 AOP 가 적용되는 순서에 대해서 이야기해주실 수 있나요?

Filter 는 Servlet Dispatcher 에 HTTP Request 가 전달되기 전에 적용되고, Interceptor 는 Servlet Dispatcher 를 통해 Controller 의 Handler 에 전달되기 전에 적용이 됩니다. 마지막으로 AOP 는 실제 메소드가 실행되는 순간에 적용이 됩니다.

즉 순서대로 말씀드리자면 HTTP Request 가 발생한 시점에서 Filter, Interceptor, AOP 순으로 적용이되고, 반대로 HTTP Response 가 발생하는 과정에서는 AOP, Interceptor, Filter 순으로 적용이 됩니다.
