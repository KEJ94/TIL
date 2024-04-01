# 템플릿 리터럴
기존의 ```var str = "Hello ES6"``` 와 같은 방식을 백틱(back-tick) 이라는 기호 (`)를 사용하여 정의 하는것이다.  
```js
const str = `Template literals are string literals allowing embedded expressions.
You can use multi-line strings and string interpolation features with them.
They were called "template strings" in prior editions of the ES2015 specification.`;
```
> ```\n``` 와 ```+``` 를 이용해 개행할 필요가 없다.  
```js
var language = 'Javascript';
var expression = `I love ${language}!`;
console.log(expression); // I love Javascript!
```
> 템플릿 리터럴을 이용하면 위와 같이 문자열과 변수를 함께 사용할수도 있다.  
> 추가로 간단한 연산도 할 수 있다.
