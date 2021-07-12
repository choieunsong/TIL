# JPA(Java Persistence API)
JPA는 Java ORM 기술에 대한 표준 명세로 Java에서 제공하는 API입니다.

### cf) ORM이란
  + Object Relational Mapping, 객체 - 관계 매핑
  + 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결) 해주는 것을 말한다
  + 객체 지향 프로그래밍에선 **클래스**를 사용하고, 관계형 데이터베이스는 **테이블**을 사용한다.
  + 데이터 <-- 매핑 --> object 필드

### cf) Object Persistence(영구적인 객체)를 위해 데이터를 데이터베이스로 사용
  + 그 방법엔 JDBC, Spring JDBC, Persistence Framework(Hibernate, Mybatis 등이 있다)

Spring Framework에선 이런 jpa와 jpa 구현체를 spring framework 기반으로 쉽게 사용하기 위해 spring data jpa를 제공한다.

# Hibernate
Hibernate는 JPS의 구현체 역할을 하는 java ORM 프레임워크입니다.

# QueryDSL
정적 타입으로 SQL 쿼리를 생성할 수 있게 해주는 프레임워크입니다. String으로 작성되는 쿼리는 오타나 잘못된 테이블 참조, 가독성을 떨어뜨리는 단점이 있는데 이러한 단점을 극복하기 위한 프레임워크입니다.
+ 컴파일 시점에 선언한 Entity Class 기반으로 Q Class가 생성되기 때문에 아래와 같이 Database 쿼리를 더 안전한 코드로 작성할 수 있도록 도와줍니다.
  ```java
  QCustomer customer = QCustomer.customer;
  JPAQuery query = new JPAQuery(entityManager);
  Customer bob = query.from(customer).where(customer.firstName.eq("Bob")).uniqueResult(customer);
  ```

# Lombok
class의 기본 getter, setter 메소드를 일일히 코드로 작성하지 않고 @Getter, @Setter 어노테이션을 통해 자동으로 생성해주는 라이브러리입니다.