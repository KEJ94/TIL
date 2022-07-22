# URI와 웹 브라우저 요청 흐름
- URI
- 웹 브라우저 요청 흐름  
<br><br>
# URI  
![](https://velog.velcdn.com/images/power0080/post/384ef5a8-6163-419b-b0b7-45b0a15edec9/image.PNG)
![](https://velog.velcdn.com/images/power0080/post/264e2b40-74e3-4546-989a-1e1c3bfab971/image.PNG)  
### URL 문법
https://www.google.com:443/search?q=hello&hl=ko
- 프로토콜 : https
- 호스트명 : www.google.com
- 포트 번호 : 443
- 패스 : /search
- 쿼리파라미터 : q=hello&hl=ko
  - 숫자를 넣어도 다 문자로 넘어가기 때문에 __쿼리스트링__ 으로도 불린다.
- fragment : html 내부 북마크 등에 사용, 서버에 전송하는 정보 X  
<br><br>
# 웹 브라우저 요청 흐름
![](https://velog.velcdn.com/images/power0080/post/d89e61fa-13da-4a27-b73a-5386be05a1d9/image.PNG)  
![](https://velog.velcdn.com/images/power0080/post/35016afe-db6d-4557-a838-5805522dfffe/image.PNG)
![](https://velog.velcdn.com/images/power0080/post/5135716a-82b1-432e-bafb-93b031bf74bd/image.PNG)
HTTP 응답 메시지
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423  

<html>
  <body>...</body>
</html>
```
