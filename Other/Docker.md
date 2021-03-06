## Docker 란?  
어플리케이션을 패키징할 수 있는 툴  
컨테이너 안에 Application, System tools, Dependencies 를 하나로 묶어서 다른 Server, 다른 PC 에 쉽게 배포하고 안정적으로 구동할 수 있게 도와주는 툴  
<br><br>
## Docker 컨테이너 
어플리케이션을 구동하는데 필요한 모든것들을 Docker 컨테이너 안에 담아둔다.  
"내 PC 에서는 되는데, 니 Server 에서는 안되니?" 와 같은 상황을 예방  
<br><br>
## 컨테이너 vs VM  
- VM 은 무거운 운영체제를 포함
- VM 에서 조금 경량화된 컨셉이 컨테이너  
- 컨테이너는 컨테이너 엔진이 설치된 Host OS를 공유한다.
- 컨테이너 엔진 중 가장많이 사랑받고 이용되는것이 "Docker"  
![](https://www.nakivo.com/blog/wp-content/uploads/2019/05/Docker-containers-are-not-lightweight-virtual-machines.png)  
<br><br>
## 동작 순서
- 만들고, 배포하고, 구동한다.
- 컨테이너를 만들기 위해서는 총 3가지가 필요하다.
- Dockerfile
  - 요리로 치면 레시피, 파일, 프레임워크, 라이브러리, 환경변수, 어떻게 구동해야되는지에 대한 스크립트를 포함
- Image
  - 코드, 런타임 환경, 시스템 툴, 시스템 라이브러리 실행되고있는 어플리케이션의 상태를 스냅샷 해서 이미지로 만들어둔다.
  - 이렇게 만들어진 이미지는 변경이 불가능한 불변의 상태로 볼 수 있다.
- Container
  - 잘 캡처해둔 어플리케이션 이미지를 고립된 환경에서 개별적인 파일 시스템안에서 실행할 수 있는 환경  
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIXvsj%2Fbtq6uVfptoX%2FLz1LwKnOpvEcQy8pmtmdsk%2Fimg.png)  
<br><br>
## 이미지 공유  
- 내 로컬머신에서 이미지를 만들어서 깃허브와 같은 컨테이너 레지스트리 안에 내가만든 이미지를 push 한다.
- 필요한 서버나 다른 개발자 PC 에서 내가 만든 이미지를 가지고와서 그걸 그대로 실행하면 된다.
- 그 이미지를 정상적으로 실행하기 위해서는 Docker 와 같은 컨테이너 엔진을 꼭 설치해야 한다.
![](https://t1.daumcdn.net/cfile/tistory/99684B395C9AA5A816)
