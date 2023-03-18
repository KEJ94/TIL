# Optional Chaning(```?.```)
ES2020에 추가된 자바스크립트 문법이다.  
옵셔널 체이닝 문법운 객체의 속성 값이 유효한지 검증할 수 있다.  
```?.``` 앞의 평가 대상이 ```undefined```나 ```null```이면 에러가 발생하지 않고 ```undefined``` 를 반환한다.
### 옵셔널 체이닝이 추가되기 전
```js
const userInfo = {
  address: {
    city: 'Seoul',
    postcode: '04377',
  }
};

const postcode = userInfo.address && userInfo.address.postcode;
```
> ```&&``` 연산자를 사용해서 ```userInfo``` 에 ```address``` 속성이 있는지 확인을 하고 ```postcode``` 에 접근했다.  
### 예제1 (객체)
```js
const userInfo = {
  address: {
    city: 'Seoul',
    postcode: '04377',
  }
};

const postcode = userInfo.address?.postcode;
```
> 이전 코드보다 문법적으로 깔끔하게 하위 속성 값에 접근할 수 있다.  
### 예제2 (함수)
```js
const userInfo = {
  getInfo: () => userInfo
};
userInfo.getInfo?.(); // userInfo object
userInfo.setInfo?.(); // undefined
```
> 함수의 존재 여부를 확인하고 호출할 때도 사용할 수 있다.
### 예제3 (```[]```)
```js
const userInfo = {
  address: {
    도시: '서울',
    postcode: '04377',
  }
};
const 키 = '도시';
console.log(userInfo.address?.[키]); // 서울
```
> ```.```대신 ```[]``` 를 사용해 객체의 속성에 접근도 가능하다.
