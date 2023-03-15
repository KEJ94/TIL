# 구조 분해 문법(Destructuring)
아래와 같은 문법을 __구조 분해__ 라고 한다.
```js
var obj = {
  a: 10,
  b: 20,
  c: 30
};

var { a, b, c } = obj;
console.log(a); // 10
console.log(b); // 20
console.log(c); // 30
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
