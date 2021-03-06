## JDBC 와 SQL Mapper, ORM 의 공통점은 무엇인가요?

JDBC 와 SQL Mapper, ORM 은 모두 영속성을 구현하기 위한 기술입니다. 영속성이란 런타임중에 발생하는 데이터, 즉 메모리상의 데이터를 영구적으로 파일이나 데이터베이스와 같은 저장소에 저장하는 것을 말합니다.

<br>

## JDBC 가 하는 역할은 무엇인가요?

JDBC 는 Java 진영에서 사용하는 영속성 기술로써, 데이터베이스의 Java DB Connectior 표준 인터페이스입니다. JDBC 를 구현한 DriverManager 가 DB Connection 을 생성하고 이를 통해 Java 애플리케이션에서 데이터베이스에 접근할 수 있습니다.

<br>

## JDBC 의 단점에는 어떤 것들이 있나요?

JDBC 는 SQL 문을 직접 작성해줘야 하기 때문에 반복적인 작업이 많이 필요하고 유지보수가 어렵다는 단점이 있습니다. 그리고 데이터베이스와 연결된 DB Connection 을 직접 관리해줘야한다는 단점도 존재합니다.

Spring JDBC 에서는 이러한 DB Connection 관리라는 단점을 해결하기 위해 설정파일에 DB 설정정보를 입력하면 이를 통해 DB Connection 을 생성하고 관리해줍니다. 이러한 DB 설정정보는 DataSource 라는 추상화된 객체에 저장되며 이를 통해 JdbcTemplate 을 생성해 데이터베이스에 접근할 수 있습니다.

<br>

## MaBatis 의 동작과정에 대해서 설명해주실수 있나요?

MyBatis 는 비즈니스 로직에 SQL 쿼리가 작성되는 것을 별도로 XML 파일에 분리해 저장합니다. Mapper 인터페이스를 생성해고 해당 인터페이스 사용할 메소드들을 정의합니다. XML 파일에서는 해당 인터페이스에 정의한 메소드들에 대해서 쿼리문을 작성해 매핑하게 됩니다.

그러면 애플리케이션이 로딩되는 시점에 Spring 이 해당 인터페이스에 대한 Bean 을 생성해 IoC 컨테이너에 등록해주고 개발자는 비즈니스 로직에서 해당 Bean 을 의존성 주입받아 사용하기만 하면 됩니다.

<br>

## MyBatis 의 장단점에 대해서 아시나요?

MyBatis 는 SQL 쿼리를 직접 관리하기 때문에 오는 장단점이 있습니다.

장점으로는 SQL 쿼리를 직접 관리하기 때문에 보다 성능이 최적화되고 복잡한 쿼리까지 작성할 수 있습니다. 그리고 특정 Entity 에 종속되지 않고 다양한 테이블을 조합해 결과를 반환할 수 있습니다.

반면에 단점으로는 SQL 쿼리를 직접 관리하기 때문에 데이터베이스 테이블에 변동사항이 생겼을 때 반복작업이 발생하는 등 유지보수가 어렵다는 문제가 있습니다. 뿐만 아니라 데이터베이스별로 추상화되어 있지 않기 때문에 데이터베이스 별로 다른 SQL 쿼리문을 작성할 수 있어야 한다는 문제가 있습니다.

<br>

## ORM 은 무엇인가요?

ORM 은 객체와 데이터베이스 테이블을 매핑해주는 도구입니다.

JDBC 는 SQL 쿼리문을 비즈니스 로직에 포함시켜야 하고, DB Connection 을 직접 관리해줘야 한다는 단점이 있습니다. 이렇게 되면 개발자는 점점 SQL 중심적인 개발을 하게 됩니다.

ORM 은 SQL 쿼리를 추상화해주기 때문에 직접 SQL 쿼리를 작성할 필요가 없습니다. 이로인해 개발할 때 SQL 쿼리문을 신경쓸 필요가 없고 데이터베이스의 스키마가 변경되더라도 반복적으로 SQL 쿼리를 수정할 필요 없이 Entity 에 대한 정보만 수정하면 됩니다.

그리고 객체는 어떻게하면 객체지향 적으로 설계할지에 대해서 초점을 맞추지만 데이터베이스는 데이터를 어떻게하면 잘 정규화하고 중복없이 저장할까에 초점을 맞춥니다. 이러한 차이에서 상속, 연관관계와 같은 문제가 발생하게 되는데 SQL 쿼리를 직접 작성해야하는 상황이라면 이를 해결하기 위한 작업까지 Data Access 계층에서 작업해줘야 합니다.

하지만 ORM 은 객체와 데이터베이스 테이블의 이러한 패러다임 차이를 극복하기 위한 작업까지 추상화되어 진행되기 때문에 개발자는 보다 개발에 집중할 수 있다는 장점이 있습니다.

<br>

## 그럼 ORM 의 단점은 없나요?

ORM 의 단점은 SQL 쿼리문을 개발자가 직접 작성하는 것이 아니라 추상화되어 있다는 점 때문에 발생합니다.

먼저 복잡한 쿼리문을 작성하기 어렵고, 작성하더라도 불필요한 SQL 쿼리들이 많이 발생하게 됩니다. 그에 따라서 당연히 데이터베이스 성능이 저하될 수 있습니다.

또 데이터베이스와 객체 사이에서 영속성 컨텍스트와 같은 기능을 제공하기 때문에 직접 SQL 쿼리를 작성하는 JDBC 나 SQL Mapper 에 비해서 성능이 느리다는 단점이 있습니다.

