# HTTP 기본
- 모든 것이 HTTP
- 클라이언트 서버 구조
- Stateful, Stateless
- 비 연결성(connectionless)
- HTTP 메시지  
<br><br>
# 모든 것이 HTTP  
- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML (API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
- __지금은 HTTP 시대!__  
### HTTP 역사
- HTTP/0.9 1991년 : GET 메서드만 지원, HTTP 헤더X
- HTTP/1.0 1996년 : 메서드, 헤더 추가
- __HTTP/1.1 1997년 : 가장 많이 사용, 우리에게 가장 중요한 버전__
- HTTP/2 2015년 : 성능 개선
- HTTP/3 진행중 : TCP 대신에 UDP 사용, 성능 개선  
### 기반 프로토콜
- TCP : HTTP/1.1, HTTP/2
- UDP : HTTP/3
- __현재 HTTP/1.1 주로 사용__
- HTTP/2, HTTP/3 도 점점 증가  
<br><br>
# 클라이언트 서버 구조  
- Request Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답  
<br><br>
# Stateful(상태유지), Stateless(무상태)  
### 차이점
- __상태유지__ 
  - 중간에 다른 점원으로 바뀌면 안된다. (중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야 한다.)
- __무상태__
  - 중간에 다른 점원으로 바뀌어도 된다.
  - 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
  - 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
  - 무상태는 응답 서버를 쉽게 바꿀 수 있다. __무한한 서버 증설 가능__  

### Stateless(무상태) 실무 한계
- 모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 있다.
- 무상태 예) 로그인이 필요없는 단순한 서비스 소개 화면
- __상태유지 예) 로그인__
- 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태유지 
- 상태유지는 최소한만 사용  
<br><br>
# 비 연결성(connectionless)  
이런식으로 통신하면 곤란하지 않을까? 
```
클라이언트1 > 연결 > 서버1(연결중)  
클라이언트2 > 연결 > 서버1(연결중)  
클라이언트3 > 연결 > 서버1(연결중) 
```
### HTTP는 비 연결성 모델
- HTTP는 __연결을 유지하지 않는 모델__
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 적음
- 서버 자원을 매우 효율적으로 사용할 수 있음  
### 한계와 극복
- TCP/IP 연결을 새로 맺어야 함 - 3way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 JS, CSS, 추가 이미지 등등 수 많은 자원이 함께 다운로드
- 지금은 HTTP ```지속연결```로 문제 해결
- HTTP/2, HTTP/3에서 더 많은 최적화  
### HTTP 초기(연결, 종료 낭비)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmBEQB%2FbtrDefzj7X3%2FzK6Md7mxTbgpMOlQkkeXGk%2Fimg.png)
### HTTP 지속연결
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbW6n5i%2FbtrDdnLlnJd%2Fh5o0t9UJJn5eKMS7m21po0%2Fimg.png)  
### Stateless를 기억하자
- 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
- 예) 선착순 이벤트, 명절 KTX 예약, 학과 수업 등록
- 예) 저녁 6:00 선착순 1000명 치킨 할인 이벤트  
<br><br>
# HTTP 메시지  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc8GMAE%2FbtrDcEfltJ8%2FR40s5hCmo3r7x9jMzddslk%2Fimg.png)  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGiXAT%2FbtrDfDlWqbv%2FWy6AIq6gqcpMnKGnk6toF0%2Fimg.png)  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQyrPE%2FbtrDfB9wjOI%2Fs1kpv88AtR9IijnhNSNUkk%2Fimg.png)   
<br><br>
