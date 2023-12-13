# Base64(64진법)
__인코딩__ 은 데이터 형태를 표준화, 보안, 처리 속도 향상, 저장 공간 절약 등을 위해서 다른 형태로 변환하는 것을 말한다.  
이메일 전송, 동영상이나 이미지 영역에서 많이 사용되며, 반대말은 __디코딩__ 이다.  
__Base64__ 란 __64진법__ 이라는 뜻으로 실행 파일이나, ZIP 파일 등 문자 코드에 영향을 받지 않는 공통 ASCII 영역의 문자들로만 바꾸는 인코딩 방식이다.  
<br/>
![](https://t1.daumcdn.net/tistoryfile/fs13/4_tistory_2009_03_22_22_26_49c63c9da9e37?original)

Man 이라는 세 글자 단어를 Base64로 인코딩 한다면 아래와 같이 변환된다.
```
원본 문자열 > ASCII binary > 6bit > base64 인코딩  
Man > 77 97 110 > 01001101 01100001 01101110 > TWFu 
```  
## 사용이유
전송해야될 데이터의 크기가 약 33%정도 늘어나고, 인코딩과 디코딩의 추가 연산까지 필요한데 왜 사용할까?  
- ASCII 는 시스템 간 데이터를 전달하기에 안전하지 않다.
- Base64는 ASCII 중 제어 문자와 일부 특수문자를 제외한 64개의 __안전한__ 출력 문자만 사용한다.
- 결과적으로 통신 과정에서의 바이너리 데이터 손실을 막기위해 사용한다.