# async & await 란?
자바스크립트의 비동기 처리 패턴 중 __가장 최근에 나온 문법__ 이다.  
기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있도록 도와준다.  
```js
var user = fetchUser('domain.com/users/1');
if (user.id === 1) {
  console.log(user.name);
}
```
> ```fetchUser()```라는 메서드를 호출하면 HTTP 통신 코드이다. 위 코드는 async & await 문법이 적용된 형태라고 봐도 된다.  
### async & await 맛보기
```js
async function logName() {
  var user = await fetchUser('domain.com/users/1');
  if (user.id === 1) {
    console.log(user.name);
  }
}
```
> 콜백으로 비동기 처리 코드를 작성하지 않아도 코드의 실행 순서를 보장받을 수 있다. 우리가 처음 프로그래밍을 배웠던 그때 그 사고로 돌아가는 것이다.
### async & await 기본 문법
```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```
> 먼저 함수의 앞에 ```async``` 라는 예약어를 붙인다. 그리고 HTTP 통신을 하는 비동기 처리 코드 앞에 ```await``` 를 붙인다.
> 여기서 주의해야할 점은 __비동기 처리 메서드가 꼭 프로미스 객체를 반환해야__ ```await``` 가 의도한 대로 동작한다.
### 간단한 예제 1
```js
async function logTodoTitle() {
  var user = await fetchUser();
  if (user.id === 1) {
    var todo = await fetchTodo();
    console.log(todo.title); // delectus aut autem
  }
}
```
> 콜백이나 프로미스보다 훨씬 간결하다.
### 간단한 예제 2
```js
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title); // delectus aut autem
    }
  } catch (error) {
    console.log(error);
  }
}
```
> async & await 예외처리 방법은 __try catch__ 이다.
