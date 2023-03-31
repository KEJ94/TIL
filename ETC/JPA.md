#### ORM 이란?
- Object-Relational Mapping  
- 객체와 관계형 DB를 맵핑한다는 뜻  
- ORM 프레임워크는 객체와 테이블을 맵핑해 패러다임의 불일치를 개발자 대신 해결해준다.    
- 객체는 객체대로 생성하고, DB는 DB에 맞도록 설계를 가능하게 해준다.
- 개발자는 이를 맵핑하는 방법만 전달해주면 된다.  
- Mybatis 는 SQL Mapper 이다. (ORM 이 아님)  
<br><br>  
# JPA
- Java Persistence API는 자바의 ORM 기술의 표준이다.  
- Java ORM 에 대한 인터페이스의 모음이다. 
- 구현체가 없고, 사용하기 위해서는 ORM 프레임워크를 선택해야 한다.  
- 다양한 프레임워크가 존재하지만 가장 대중적인 것은 __하이버네이트__ 이다.  
<br><br>  
## 동작 과정
![](https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-insert-structure.png)  
JPA는 애플리케이션과 JDBC 사이에서 동작한다.  
__JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.__  
개발자가 ORM 프레임워크에 저장하면 __적절한 INSERT SQL__ 을 생성해 데이터베이스에 저장해주고,  
검색을 하면 __적절한 SELECT SQL__ 을 생성해 결과를 객체에 매핑하고 전달해준다.  
<br><br>  
## 사용 이유  
### 1. 생산성  
JPA를 사용하면 Java 컬렉션에 저장하듯이 JPA에게 저장할 객체를 전달하면 된다.  
지루하고 반복적인 코드를 개발자가 직접 작성하지 않아도 되며, DDL문도 자동으로 생성해주기 때문에 데이터베이스 설계 중심을 객체 설계 중심으로 변경할 수 있다.  
<br>
### 2. 유지보수
필드를 하나만 추가해도 관련된 SQL과 JDBC 코드를 전부 수정해야 했지만 JPA는 이를 대신 처리해주기 때문에 개발자가 유지보수 해야하는 코드가 줄어든다.  
<br>
### 3. 패러다임의 불일치 해결
JPA는 연관된 객체를 사용하는 시점에 SQL을 전달할 수 있고, 같은 트랜잭션 내에서 조회할 때 동일성도 보장하기 때문에 다양한 패러다임의 불일치를 해결한다.  
<br>
### 4. 성능
애플리케이션과 DB사이에서 성능 최적화 기회를 제공한다.  
같은 트랜잭션안에서는 같은 엔티티를 반환하기 때문에 DB와의 통신횟수를 줄일 수 있다. 또한, 트랜잭션을 commit 하기전까지 메모리에 쌓고 한번에 SQL을 전송한다.  
<br>
### 5. 데이터 접근 추상화와 벤더 독립성
RDB는 같은 기능이라도 벤더마다 사용법이 다르기 때문에 처음 선택한 DB에 종속되고 변경이 어렵다.  
JPA는 애플리케이션과 DB 사이에서 추상화된 데이터 접근을 제공하기 때문에 종속이 되지 않도록 한다.  
만약 DB가 변경되더라도 JPA에게 알려주면 간단하게 변경이 가능하다.  
<br>







