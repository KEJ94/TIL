## Flyway
flyway는 데이터베이스의 형상관리를 목적으로 하는 툴 이다.

<br><br>
## 마이그레이션 파일
flyway에서는 데이터베이스에 일어나는 모든 행위를 마이그레이션(migration) 이라고 표현 한다.  
이러한 마이그레이션은 파일로 관리되어 진다.  
파일의 이름은 지정하는 형식이 있으며 이를 따라야한다.  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcqvO9u%2FbtrfgYIhw2u%2FkKyRbZFhXrOt1T1YYuCgS0%2Fimg.png)  
- Prefix : V, U, R 중 하나를 입력 V는 Version, U는 Undo, R은 Repeatable 일단은 V만 숙지하자
- Version : 버전 정보다. 정수,소수,날짜 등 가능
- Seperator : __ (underscore 2개를 이용)
- Description : 추가되는 설명 _ (underscore)가 space를 대신한다.
<br>
위 규칙으로 파일을 만들면 아래와 같이 만들 수 있다.

__V220622.001__VADA.sql__  
__V1__VADA.sql__  

파일명 외 세팅이 정상일 경우 Spring Boot 배너가 뜨면서 DbMigrate 로깅을 확인할 수 있다.

<br><br>
## flyway_schema_history 테이블
해당 테이블은 flyway 에서 형상관리를 위하여 자동으로 만드는 테이블이다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpDNJE%2FbtrfjWIKdHQ%2FKdEEAaW34kBzC3o62onmFk%2Fimg.png)  
눈여겨 봐야할 정보는 version, checksum, success 이다.
- version은 파일의 V 뒤에 붙어있던 숫자로 낮은 순서부터 실행되며 실행 순서대로 테이블에 쌓이는 구조
- checksum은 파일의 내용을 hashing 한 것, 만약 파일의 내용이 달라지면 이 체크섬이 달라지게 된다. __한번 체크섬을 만들어 둔 후 파일을 수정한다면 누군가에 의해서 형상관리에 문제가 생겼다고 판단하기 때문에 flyway는 에러처리를 하게된다.__ 이럴 경우 해당 파일에 대한 체크섬을 repair한 후 success를 0으로 돌리는등의 작업이 필요하다.
- success는 파일 실행에 성공여부를 나타내는 값이다. 이 값에 따라서 flyway에서 해당 파일의 내용을 실행할지 말지 정한다.
<br><br>
## 발생할 수 있는 에러
Spring Boot가 실행되면 가장 먼저 flyway가 실행하게 된다. 이때 발생할 수 있는 에러에 대해 알아보자
- 가장 많이 발생하는 에러는 한번 flyway 로 서비스를 모두 올린 후 일부 파일을 수정하여 checksum이 틀어지게 되어 발생하는 에러다. 해결 방법으로 2가지가 있다.
  - 첫번째 : 해당 row를 포함한 이후 row를 삭제하고 다시 올리는방법
  - 두번째 : checksum을 강제로 수정하고(repair 또는 수동으로 수정) success를 1로 돌리는 방법

<br><br>
## REPAIR
repair는 checksum이 틀어지는 등의 문제가 생겼을 때 자동으로 checksum을 맞춰주는 방법이다.
flyway 6 에서는 bean 을 아래처럼 등록하면 migrate 하기전 repair를 먼저하여 checksum 을 재설정하는 작업을 진행하여 형상이 깨지는것을 막아준다.
```
@Bean
public FlywayMigrationStrategy cleanMigrateStrategy() {
    return flyway -> {
        flyway.repair();
        flyway.migrate();
    };
}
```

<br><br>
## 알아두면 좋은 점
하나의 파일에 2개의 DDL은 좋지 않다. flyway는 checksum을 파일 단위로 관리하며 파일에 문제가 생겼을 때 이 이후의 로직을 실행하지 않기 때문에 DDL은 파일 하나씩 두는것을 추천한다.
<br><br>

