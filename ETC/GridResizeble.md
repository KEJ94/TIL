## 사용자가 그리드 크기를 조절할 수 있는 테이블

### SCSS
```scss
.grid-resizeble table { // 테이블 부모요소에 grid-resizeble 클래스를 준다.
    table-layout: fixed;
    user-select: none;
    td {
        overflow: hidden; // 크기가 초과될 경우 보이지 않도록 설정
        text-overflow: ellipsis; // overflow 가 선언된 상태에서만 작동한다. 초과한 텍스트를 ... 으로 변환
        white-space: nowrap;
    }
    .tw-10 { 
        width: 10px; 
    }
    .tw-20 {
        width: 20px;
    }
    // ... 필요한 사이즈별로 추가로 작성한다.
}
```
> Vuetify.js 의 v-treeview 를 사용했기 때문에 데이터에 ```thClass: 'tw-120'``` 처럼 ```th``` 에 지정할 클래스 style 을 미리 준비한다.
### JS
```js
    GRID_RESIZABLE(){
        var thElm;
        var startOffset;
        Array.prototype.forEach.call(
        document.querySelectorAll("table th"),
        (th) => {
            th.style.position = 'relative';
            var grip = document.createElement('div');
            grip.innerHTML = "&nbsp;";
            grip.style.top = 0;
            grip.style.right = 0;
            grip.style.bottom = 0;
            grip.style.width = '5px';
            grip.style.position = 'absolute';
            grip.style.cursor = 'col-resize';
            grip.addEventListener('mousedown', (e) => {
                thElm = th;
                startOffset = th.offsetWidth - e.pageX;
            });
            th.appendChild(grip);
        });
        document.addEventListener('mousemove', (e) => { 
            if(thElm){
                const width = startOffset + e.pageX;
                if(width > 100){
                    thElm.style.width = width + 'px';
                }
            }
        });
        document.addEventListener('mouseup', () => thElm = undefined);
    }
```
> 테이블에 마우스 over, up 등 이벤트를 지정한다.
### 설명
처음에는 ```th``` 에도 ```overflow: hidden;``` 설정을 주다가 헤더에 포함된 자식요소 (div) 가 잘리는 상황 발생  
```td``` 에만 적용하고 ```th``` 최소길이를 100px; 로 조정 
