# 자바스크립트 함수의 this 바인딩
자바스크립트의 this는 상황에 따라 다르게 바인딩 된다. 이상하게도 함수 호출 시 함수 내부의 this 는 지정되지 않는다.  
```javascript
const cat = {
  name: 'meow',
  foo1: function() {
    const foo2 = function() {
      console.log(this.name);
    }
    foo2();
  }
};

cat.foo1();	// undefined
```
- 위 예제는 ```cat.foo1()``` 함수 호출 시 내부 함수 foo2 가 실행되는 구조이다.
- 함수가 호출됐으므로 ```foo2``` 내부의 this는 지정되지 않아서 곧 전역 객체를 가리키게 된다.
- 전역 객체에 ```name```이란 속성은 존재하지 않으므로 ```undefined``` 로 출력된다.

위와 같은 문제를 해결할 때 __화살표 함수__ 를 사용하면 해결된다.  
```javascript
const cat = {
  name: 'meow',
  foo1: function() {
    const foo2 = () => {
      console.log(this.name);
    }
    foo2();
  }
};

cat.foo1();	// meow
```
> 이게 가능한 이유는 화살표 함수에는 __this가 아예 없기 때문__ 이다. 즉 ```function``` 으로 선언한 함수를 실행할 땐 this가 존재하긴 하지만 값을 지정하지 않는데, 화살표 함수로 선언한 함수에는 this가 없다.  
> JavaScript 에서는 어떤 식별자(변수)를 찾을 때 현재 환경에서 __그 변수가 없으면 바로 상위 환경을 검색__ 하게된다.  
> 그렇게 점점 상위 환경으로 타고 타고 올라가다가 변수를 찾거나 가장 상위 환경에 도달하면 그만두게 되는데, __화살표 함수의 this 바인딩 방식도 이와 유사하다!__  
> 화살표 함수에는 __this라는 변수 자체가 존재하지 않기 때문에__ 그 상위환경에서의 this를 참조하게 된다.  

### 이럴 땐 화살표 함수를 쓰면 안된다.
```javascript
const cat = {
  name: 'meow';
  callName: () => console.log(this.name);
}

cat.callName();	// undefined
```
> 이 같은 경우, ```callName``` 메서드의 ```this```는 자신을 호출한 객체 ```cat```이 아니라 함수 선언 시점의 상위 스코프인 전역객체를 가리키게 된다. 어차피 일반 함수를 사용해도 메서드로 호출하면 자신을 호출한 객체를 가리키기 때문에 메서드에서 화살표 함수를 쓸 필요는 없다.

### 마무리
이렇게 화살표 함수를 사용하는 이유는 자바스크립트의 이상한 this 때문이다. (추가로 한 코드 작성이 가능해진다.)






