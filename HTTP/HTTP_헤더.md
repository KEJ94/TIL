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

구글에서 검색 후 ```Network - Headers - Request Headers``` 확인해보자  
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
용량이 줄어들고 뭘로 압축되어있는지 (gzip) 알려주고 보낸다.
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
분할전송때는 Content-Length 를 보내지 않는다. 
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





