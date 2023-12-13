# 싱글톤패턴
생성자가 여러번 호출되더라도 __실제로 생성되는 객체는 하나__  이다.  

### 사용 이유
요청이 엄청 많은 사이트에서 계속 객체를 생성하게 되면 __(heap) 메모리 낭비__ 가 심하기 때문이다.

### 구현 예제
```.java
// 1. static 영역에 객체를 딱 1개만 생성한다.
private static final SingletonService instance = new SingletonService();

// 2. static method 를 통해서만 객체를 생성하도록 한다.
public static SingletonService getInstance() {
    return instance;
}

// 3. private 생성자를 통해서 외부에서 new 생성을 막는다.
private SingletonService() {} 
```
### 사용 예제
```.java
SingletonService singletonService1 = SingletonService.getInstance();
SingletonService singletonService2 = SingletonService.getInstance();
Assertions.assertThat(singletonService1).isSameAs(singletonService2);
```
### 스프링 컨테이너
스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리한다.  
이러한 기능 덕분에 싱글톤 패턴의 모든 단점을 해결하고 객체를 싱글톤으로 유지할 수 있다.  

### 주의사항
객체 인스턴스를 공유하기 때문에 객체 상태를 __유지(stateful)__ 하게 설계하면 안된다.
