# 기본값 매개변수(Default parameter)
함수의 매개변수에 값이 전달되지 않을 때 기본 값으로 설정하는 문법이다.  
```null``` 도 정의할 수 있다. ```undefined``` 값인 경우에만 기본값을 할당한다.  
### 예제1
```js
function foo(param1 = 1, param2 = {}, param3 = 'korean') {
  console.log(param1, param2, param3);
};
foo(30, { name: 'amy' }, 'american'); // 30, { name: 'amy' }, 'american'
```
### 예제2
```js
function printFruit(name = 'apple', weight = 10, price = 15000) {
  console.log(name, weight, price);
};
printFruit(0, false, null); // 0, false, null
printFruit(); // apple, 10, 15000
```
