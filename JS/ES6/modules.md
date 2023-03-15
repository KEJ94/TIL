# Import & Export
이제는 자바스크립트 언어 자체에서 모듈화를 지원한다.  
### 기본 문법
```js
export 변수, 함수
import {불러올 변수 또는 함수 이름} from '파일 경로';
```
### 기본 예제
```js
// math.js
export var pi = 3.14;
export function sum(a, b) {
  return a + b;
}
```
```js
// app.js
import { pi, sum } from './math.js';
console.log(pi); // 3.14
sum(10, 20); // 30
```
### 브라우저 지원 범위
ES6의 기본적인 문법들이 최신 브라우저에서 지원되는데 반해 import, export 는 아직 보조도구가 있어야만 사용할 수 있다.  
가급적 실무 코드에서 사용할 때는 __웹팩(Webpack)__ 과 같은 __모듈 번들러__ 를 이용하여 구현해야 한다.
