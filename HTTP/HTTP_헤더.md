## HTTP 헤더
```
Content-Type: text/html;charset=UTF-8
Content_Length: 3423
```
- HTTP 전송에 필요한 모든 부가정보를 포함
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등등
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능  
<br><br>
## HTTP 표준
- 1999년 RFC2616 (폐기)
- 2014년 RFC7230~7235 등장  
<br><br>
## HTTP BODY (message body - RFC7230 최신)
```
HTTP/1.1 200 OK
-----------------------------------------
// 표현 헤더
Content-Type: text/html;charset=UTF-8
Content-Length: 3423
-----------------------------------------
// 표현 데이터 or 메시지 본문
<html>
  <body>...</body>
</html>
-----------------------------------------
```
- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- __표현__ 은 요청이나 응답에서 전달할 실제 데이터
- __표현 헤더는 표현 데이터__ 를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등  
<br><br>
## 표현
### Content-Type 표현 데이터의 형식  
- 미디어 타입, 문자 인코딩
  - text/html; charset=utf-8
  - application/json
  - image/png  
### Content-Encoding 표현 데이터의 압축 방식
- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - gzip
  - deflate
  - identity  
 ### Content-Language 표현 데이터의 자연 언어
- 표현 데이터의 자연 언어를 표현
  - ko
  - en
  - en-US  
### Content-Length 표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding(전송코딩)을 사용하면 Content-Length를 사용하면 안됨  
<br><br>
## 컨텐츠 협상
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청시에만 사용

### Accept-Language 적용전 한국어 브라우저 사용
클라이언트 요청
```
GET /event
Accept-Language : ko
```
다중 언어 지원 서버 응답
```
Content-Language : ko
안녕하세요
```  

### 협상과 우선순위 1
- Quality Values(q) 값 사용
- 0~1 클수록 높은 우선순위
- 생략하면 1
- Accept-Language : ko-KR,ko;q=0.9,en-US;q=0.8,eb;q=0.7
 1. ko-KR;q=1 (q생략)
 2. ko;q=0.9
 3. en-US;q=0.8
 4. en;q=0.7  

### 협상과 우선순위 2
- 구체적인 것이 우선이다.
- Accpet : text/*, text/plain, text/plain;format=flowed, /*
 1. text/plain;format=flowed
 2. text/plain
 3. text/*
 4. /*

구글(Chrome)에서 검색 후 ```F12 - Network - Headers - Request Headers``` 확인해보자  
<br><br>
## 전송 방식
### 단순 전송 (Content-Length)
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

<html>
 <body>...</body>
</html>
```
3423 바이트로 보낸다고 미리 알려주고 한번에 전송, 한번에 받는다.
### 압축 전송 (Content-Encoding)
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Length: 521

qwdion102e120n1029rn1203120nd012nd0n10d229
```
용량이 줄어들고 어떻게 압축되어있는지 (gzip) 알려주고 보낸다.
### 분할 전송 (Transfer-Encoding)
```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

5
Hello
5
World
0
\r\n
```
분할 전송때는 Content-Length 를 보내지 않는다. 
### 범위 전송 (Range, Content-Range)
```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Range: bytes 1001-2000 / 2000

qoiwdnqoido1ind1029dn102dn1290dn9012nd012n
```
이미지를 받는데 도중에 끊긴 경우 다시 요청하면 용량이 아깝기 때문에 범위를 지정해서 요청할 수 있다.  
<br><br>
## 일반 정보
### From (유저에이전트의 이메일 정보)
- 일반적으로 잘 사용되지 않음
- 검색엔진 같은 곳에서 주로 사용
- 요청에서 사용
### Referer (이전 웹페이지 주소)
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A > B로 이동하는 경우 B를 요청할 때 Referer: A 를 포함해서 요청
- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용
- 참고: referer는 단어 referrer의 오타
### User-Agent (유저 에이전트 애플리케이션 정보)
- user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36(KHTML, like Gecko) Chrome/86.0.0.4240.183 Safari/537.36
- 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용
### Server (요청을 처리하는 ORIGIN 서버의 소프트웨어 정보)
- ORIGIN 서버란? HTTP 요청을 보내면 중간의 여러 프록시 서버를 거치게된다. 그 끝에있는 서버를 ORIGIN 서버라 한다.
- Server: Apache/2.2.22 (Debian)
- server: nginx
- 응답에서 사용
### Date (메시지가 발생한 날짜와 시간)
- Date: Tue, 15 Nov 1994 08:12:31 GMT
- 응답에서 사용  
<br><br>
## 특별한 정보
### Host (요청한 호스트 정보 - 도메인)
- 요청에서 사용
- 필수
  - 하나의 서버가 여러 도메인을 처리해야 할 때 필요
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 필요
  - 가상호스트를 통해 여러 도메인을 한번에 처리할 수 있는 서버에서 필요
  - ```IP:200.200.200.2``` 서버에 aaa.com, bbb.com, ccc.com 의 어플리케이션이 구동될 수 있다.
### Location (페이지 리다이렉션)
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
- 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
- 3xx (Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킴
### Allow (허용 가능한 HTTP 메서드)
- 405 (Method Not Allowed) 에서 응답에 포함해야한다.
- Allow: GET, HEAD, PUT
### Retry-After (유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간)
- 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
- Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
- Retry-After: 120 (초단위 표기)  
<br><br>
## 인증
### Authorization (클라이언트 인증 정보를 서버에 전달)
- Authorization: Basic xxxxxxxxxx
### WWW-Authenticate (리소스 접근시 필요한 인증 방법 정의)
- 리소스 접근시 필요한 인증 방법 정의
- 401 Unauthorized 응답과 함께 사용
- WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"", Basic realm="simple"  
<br><br>
## 쿠키
![](https://velog.velcdn.com/images%2Fshkim1199%2Fpost%2Fb1cd9d31-dbe3-443e-8ec1-7d6604daab5b%2Fimage.png)  
웹 브라우저 내부에 있는 쿠키 저장소에 저장 후 로그인 이후에 자동으로 쿠키를 꺼내본다.
```
GET /welcome HTTP/1.1
Cookie: user=홍길동
```
- 예) set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=google.com; Secure
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
  - 최소한의 정보만 사용(세션 id, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고
- 주의
  - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드번호 등등)  
  
### 쿠키(보안)
- Secure
  - 쿠키는 http, https를 구분하지 않고 전송
  - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP 전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송  
<br><br>
## 캐시
### 캐시가 없을때
- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운받아야 한다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리다.
- 느린 사용자 경험
__캐시가 있다면? 메모리 아니면 하드디스크에서 불러오게 되어 빠르다.__


### 캐시 시간 초과
캐시 유효시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타난다.
- 서버에서 기존 데이터를 변경함
- 서버에서 기존 데이터를 변경하지 않음
  - 캐시 유효시간이 초과해도, 서버의 데이터가 갱신되지 않으면?
  - 304(수정되었으면 200) Not Modified + 헤더 메타 정보만 응답(바디X)
  - 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
  - 클라이언트는 캐시에 저장되어 있는 데이터 재활용
  - __결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드 (매우 실용적인 해결책)__
  - Chrome F12 Network 의 .png 같은 파일의 이름이 흐린 파일을 클릭하고 Headers를 확인해보자

### 검증 헤더와 조건부 요청
- 검증 헤더
  - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  - __Last-Modified, ETag__
- 조건부 요청 헤더
  - 검증 헤더로 조건에 따른 분기
  - If-Modified-Since: __Last-Modified__ 사용
  - If-None-Match: __Etag__ 사용
  - 조건이 만족하면 200 OK
  - 조건이 만족하지 않으면 304 Not Modified

### Last-Modified 단점
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
  - A > B 로 바꿨다가 B > A 로 다시 바꿔서 같아진 경우
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고싶은 경우

### ETag
- 캐시용 데이터에 임의의 고유한 버전 이름을 달아둔다.
  - 예) ETag: "v1.0", ETag: "a2idno1"
- 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
  - 예) ETag: "aaaaa" > ETag: "bbbbb"
- 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기!






