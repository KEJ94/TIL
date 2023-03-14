# 향상된 객체 리터럴(Enhanced Object Literal) 
기존 자바스크립트에서 사용하던 __객체 정의 방식__ 을 개선한 문법  
### 방식 1 - 속성과 변수명이 같으면 1개만 기입
```js
var language = 'javascript';
var josh = {
  // language: language,
  language
};
console.log(josh); // {language: "javascript"}
```
### 방식 2 - 속성에 함수를 정의할 때 function 예약어 생략
```js
const josh = {
  coding() { // coding: function() {
    console.log('Hello World');
  }
};
josh.coding(); // Hello World
 
```
