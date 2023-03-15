# Nullish coalescing operator(```??```)
널 병합 연산자는 연산자(```??```)의 왼쪽 피연산자가 __null__ 또는 __undefined__ 일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자 이다. 
### 기존방식
```js
let tilte = text;
if (text == null || text == undefined) {
  title = '타이틀';
}
```
### 널 병합 연산자를 이용한 방식
```js
let title = text ?? '타이틀';
```
### 널 병합(```??```) vs OR(```||```) 두 연산자의 차이점
- __```||```__ : ```null```, ```undefined```, ```0```, ```''```, ```NaN``` 의 경우 오른쪽 피 연산자를 반환
- __```??```__ : ```null```, ```undefined``` 의 경우 오른쪽 피 연산자를 반환
