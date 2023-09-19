# 싱글톤패턴
생성자가 여러번 호출되더라도 __실제로 생성되는 객체는 하나__  이다.

### 사용 이유
요청이 엄청 많은 사이트에서 계속 객체를 생성하게 되면 (heap) 메모리 낭비가 심하기 때문이다.

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
