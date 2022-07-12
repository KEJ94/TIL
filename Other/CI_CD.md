## CI/CD란?
요즘같이 빠르게 진화하고 변화하는 시대에 어떻게 하면 시장과 고객의 요구에 빠르게 반응해서 제품을 출시, 업데이트 할 것인가?  
이것을 위해서 세계적으로 많은 기업들이 __CI/CD__ 를 개발 프로세스로 사용하고 있다.  
어플리케이션 __개발 ~ 배포__ 까지의 모든 단계를 자동화를 통해서 조금 더 효율적이고 빠르게 사용자에게 빈번히 배포할 수 있도록 한다.  

### CI (지속적인 통합)
- 코드변경사항을 주기적으로 빈번하게 머지해야 한다.
  - dev1, dev2 개발자 둘이 a.js 파일을 서로 한달동안 작업하게되면 머지하는데 시간이 오래 걸리게 된다.
  - 최대한 작은단위로 나누어서 개발하고 통합하는것이 중요하다.
- 통합을 위한 단계의 자동화
  - Merge > Build > Test
- 버그 수정 용이
  - 문제점을 빠르게 발견  

### CD (지속적인 제공 or 배포)
- 어떻게 하면 자동화 해서 배포를 만들 수 있을지 고민하는 단계
- 지속적인 제공 (Continuous Delivery)
  - CI(Build, Test) 
  - Prepare Release(릴리즈할 준비 단계)
  - __수동적으로 검증 > DEPLOY RELEASE__
- 지속적인 배포 (Continuous Deployment) 
  - CI(Build, Test) 
  - Prepare Release(릴리즈할 준비 단계)
  - __자동적으로 검증 > DEPLOY RELEASE__

### Tool
- Jenkins
- Buildkite
- Github Actions
- GitLab CI/CD 
