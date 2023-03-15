# 구조 분해 문법(Destructuring)
기존 자바스크립트의 __구조__ 는 아래와 같다.
```js
var arr = [1, 2, 3, 4];
var obj = {
  a: 10,
  b: 20,
  c: 30
};
```
아래와 같은 문법을 __구조 분해__ 라고 한다.
```js
var { a, b, c } = obj;
```
### 객체의 속성을 가져올 때
```js
var josh = {
  language: 'javascript',
  position: 'front-end',
  area: 'pangyo',
  hobby: 'singing',
  age: '102'
};

// 기존
var language = josh.language;
var position = josh.position;
var area = josh.area;
var hobby = josh.hobby;
var age = josh.age;

// ES6+
var { language, position, area, hobby, age } = josh;
console.log(language); // javascript
console.log(position); // front-end
console.log(area); // pangyo
console.log(hobby); // singing
console.log(age); // 102
```
### 뷰엑스 1
```js
// 기존
actions: {
  fetchData(context) {
    context.commit('addProducts');
  }
}

// ES6+
actions: {
  fetchData({ commit }) {
    commit('addProducts');
  }
}
```
### 뷰엑스 2
```js
var context = {
  commit: actionName => console.log(actionName +' has been committed!!')
};
var { commit } = context;
commit('addProducts'); // addProducts has been committed!!  
```
### 뷰엑스 3
```js
// 기존
<li v-for="post in posts" :key="post.id">
  {{ post.title }} - {{ post.author }}
</li>

// ES6+
<li v-for="{ title, author, id } in posts" :key="id">
  {{ title }} - {{ author }}
</li>
```
