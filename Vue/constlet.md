# const & let
const 와 let 예약어는 ES6에서 사용하는 변수 선언 방식이다.
### let
let 예약어는 한번 선언하면 다시 선언할 수 없다.
```js
// 똑같은 변수를 재선언할 때 오류 
let a = 10;
let a = 20; // Uncaught SyntaxError: Identifier 'a' has already been declared
```
### const
const 예약어는 한번 할당한 값을 변경할 수 없다.
```js
// 값을 다시 할당했을 때 오류
const a = 10;
a = 20; // Uncaught TypeError: Assignment to constant variable.
```
> 단, 객체 ```{}``` 또는 배열 ```[]``` 로 선언했을때는 객체의 속성과 배열의 요소를 변경할 수 있다.
### var 예약어와 차이
const & let 은 블록 유효범위가 다르다.
```js
var a = 10;
for (var a = 0; a < 5; a++) {
    console.log(a); // 0 1 2 3 4 5
}
console.log(a); // 6
```
```js
var a = 10;
for (let a = 0; a < 5; a++) {
    console.log(a); // 0 1 2 3 4 5
}
console.log(a); // 10
```
> 반복문의 조건 변수 a 를 let 으로 선언하니 변수의 유효 범위가 for 문의 ```{}``` 블록 안으로 제한되었다.
