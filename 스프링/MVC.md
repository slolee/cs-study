## MVC 패턴에 대해서 설명해주실수 있나요?

MVC 패턴이란 애플리케이션을 역할에 따라서 Model, View, Controller 로 나눠 구성하는 아키텍처입니다. MVC 패턴의 주 목적은 사용자에게 인터페이스를 제공하기 위한 View 와 데이터를 처리하는 비즈니스 로직, 그리고 데이터베이스에 접근해 사용자에게 보여줄 데이터를 다루기 위한 로직을 분리하기 위함입니다.

Controller 가 사용자의 요청을 받고 Service 와 함께 비즈니스 로직을 처리한 뒤 결과를 Model 에 담고 Model 을 통해 View 를 생성합니다. 이렇게 생성한 View 를 사용자에게 반환하는 흐름으로 진행되는 것을 MVC 패턴이라고 합니다.

<br>

## JSP 와 Servlet 의 차이점에 대해서 설명해주세요.

JSP 와 Servlet 모두 Java 에서 웹 애플리케이션을 구현하기 위한 도구들입니다.

JSP 는 HTML 문서 내부에 Java 문법을 사용할 수 있도록 해주는 것으로, Servlet 만으로 사용자에게 보여줄 View 를 만드는 것이 매우 까다로워 등장한 서버 스크립트 언어입니다.

Servlet 은 Java 파일에서 HTML 코드를 삽입할 수 있도록 해주는 것으로, 클라이언트로부터 요청을 받아 서버에서 동적인 컨텐츠를 생성하기 위해 사용됩니다.

일반적으로 Servlet 은 클라이언트의 요청을 받아 처리하는 Controller 를 구현하는데 사용되고, JSP 는 Model 에 포함된 데이터를 이용해 사용자에게 보여줄 View 를 만들기 위해서 사용되는 것으로 알고 있습니다.

<br>

## MVC 모델1 과 모델2의 차이점에 대해서 아시나요?

MVC 모델1 은 Servlet 없이 JSP 만으로 모든 요청을 처리하는 구조로 알고 있습니다. JSP 에 비즈니스 로직을 처리하는 Java 코드를 포함해 개발하게 됩니다. 즉, View 와 Controller 가 결합되어있는 구조로 설명할 수 있을 것 같습니다.

반면에 MVC 모델2 는 View 와 Controller 를 분리해 클라이언트의 요청을 받아 비즈니스 로직을 수행하는 Controller 는 Servlet 으로 구현하고, 사용자에게 보여줄 View 는 JSP 로 구현하도록 하는 아키텍처입니다.

```MVC 모델1 보다 설계에 대한 요구사항은 증가하지만 View 와 비즈니스 로직을 분리함으로써 코드의 재사용성이 높아지고 확장성, 유지보수가 편리해진다는 장점이 있습니다.```

<br>

## Spring 으로 구현된 웹 애플리케이션으로 요청이 들어온다고 했을 때 클라이언트에게 응답이 갈때까지의 흐름에 대해서 설명해주세요.

클라이언트의 HTTP 요청이 오게되면 가장먼저 Filter Chain 을 거치게 되며, 이후에 Dispatcher Servlet 으로 요청이 도착하게 됩니다.
Dispatcher Servlet 은 HTTP 요청을 Handler Mapper 에 전달해 적절한 Controller 의 Handler 를 선택하고 해당 Handler 에 HTTP 요청을 보내게 됩니다.
내부적으로 비즈니스 로직을 수행하고 그 결과 데이터를 Model 에 담아 Dispatcher Servlet 으로 응답하고 Dispatcher Servlet 은 이를 View Resolver 에게 전달해 View 를 생성합니다.
이렇게 생성한 View 를 Dispatcher Servlet 이 클라이언트에게 응답을 보내는 흐름으로 진행되는 것으로 알고 있습니다.

<br>

## Dispatcher Servlet 이 왜 존재하는지 아시나요?

Spring 에서 Dispatcher Servlet 이 없을 때는 Servlet 객체를 모두 web.xml 파일에 일일히 등록을 해줘야됐습니다. Dispatcher Servlet 은 모든 컨트롤러의 최앞단의 프론트 컨트롤러로써 클라이언트의 모든 요청을 받아 적절한 컨트롤러의 Handler 에 전달해주는 역할을 수행합니다.

Dispatcher Servlet 을 사용함으로써 얻을 수 있는 이점은,
첫 번째로 Servlet 객체를 web.xml 에 일일히 등록하지 않아도 된다는 점입니다.
두 번째로 Servlet 객체를 사용하는 것이 아니라 @Controller 어노테이션을 붙인 컨트롤러 클래스를 통해 Handler 를 구현한다는 점입니다. 기존에 Servlet 객체는 HttpServlet 클래스를 구현하는 클래스이고 다시 말하면 Servlet 이라는 라이브러리에 의존하는 객체입니다. Dispatcher Servlet 을 사용함으로써 Controller 클래스는 Servlet 라이브러리와 분리되어 POJO 객체로 사용이 되게 됨으로써 결합도가 낮아진다는 장점이 있습니다.

예를 들어, Servlet Container 가 아닌 Netty 와 같은 것으로 WAS 를 변경한다고 했을 때 Servlet 객체로 작성이 되어있다면 이 부분을 모두 수정해야 합니다. 하지만 Dispatcher Servlet 을 이용하게 되면 Controller 는 수정하지 않고 앞단만 변경해주면 된다는 점에서 결합도가 낮다고 말할 수 있을 것 같습니다.

마지막으로 모든 End Point 로 가는 클라이언트 요청에 대해서 공통적으로 처리해야하는 작업이 있다면 모든 요청이 거쳐 지나가는 Dispatcher Servlet 을 통해 처리할 수 있습니다.