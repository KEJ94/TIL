# 인스턴스  
인스턴스는 뷰로 개발할 때 필수로 생성해야 하는 코드다.  
### 인스턴스 생성
```js
new Vue();
```
> ```console.log``` 로 확인해보면 인스턴스 안에는 미리 정의되어 있는 __속성__ 과 __메서드(API)__ 들이 있다.
### 인스턴스의 속성, API
```js
new Vue({
  el: ,
  template: ,
  data: ,
  methods: ,
  created: ,
  watch: ,
});
```
> - el : 인스턴스가 그려지는 화면의 시작점 (특정 HTML 태그)
> - template : 화면에 표시할 요소 (HTML, CSS 등)
> - data : 뷰의 반응성(Reactivity) 이 반영된 데이터 속성
> - methods : 화면의 동작과 이벤트 로직을 제어하는 메서드
> - created : 뷰의 라이프 사이클과 관련된 속성
> - watch : data에서 정의한 속성이 변화했을 때 추가 동작을 수행할 수 있게 정의하는 속성  
