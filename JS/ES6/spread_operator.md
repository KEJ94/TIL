# 스프레드 오퍼레이터(Spread Operator) ES2018 (ES9)
연산자 모양은 ```...``` 으로, 한글로 번역하면 펼침 연산자 정도로 볼 수 있다.  
객체 또는 배열의 값을 다른 객체, 배열로 복제하거나 옮길 때 사용한다.  
### 잘못된 배열 복사
```js
let arrA = [1, 2, 3];
let arrB = arrA;
arrB[0] = 10;
console.log(arrA); // [10, 2, 3]
console.log(arrB); // [10, 2, 3]
```
> 이러한 원인은 배열은 값 형식이 아닌 참조 형식이기 때문이다.
### 올바른 배열 복사(기존, ES6+)
```js
// 기존 방식
var arr1 = [1,2,3]; 
var arr2 = [4,5,6]; 
var arr = arr1.concat(arr2); 
console.log(arr); // [ 1, 2, 3, 4, 5, 6 ] 
// ES6+ spread operator
var arr1 = [1,2,3]; 
var arr2 = [4,5,6]; 
var arr = [...arr1, ...arr2]; 
console.log(arr); // [ 1, 2, 3, 4, 5, 6 ] 
```
### 뷰엑스 적용 예제
```js
import { mapGetters } from 'vuex';

export default {
  computed: {
    ...mapGetters(['getStrings', 'getNumbers', 'getUsers']),
  }
}
```
> 위 코드에서 ```mapGetters``` 라는 헬퍼 함수 앞에 ```...``` 를 사용하였다. 
> 그러지 않았다면 어떻게 될까?
```js
import { mapGetters } from 'vuex';

export default {
  computed: {
    getStrings() {
      // ..
    },
    getNumbers() {
      // ..
    },
    getUsers() {
      // ..
    },
  }
}
 
```
