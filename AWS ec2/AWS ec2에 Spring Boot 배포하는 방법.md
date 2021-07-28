# AWS ec2 인스턴스 생성해서 Spring Boot 프로젝트를 배포해보자
Kurento 서버를 Vue에서 접근하려고 하는데 죽어도 안된다;; 물어물어 찾아찾아보니 애초에 로컬호스트에서 접근이 안됐던것;;
결국 서버에 배포해서 테스트하기로 했다

> 사양 
+ Mac
+ AWS ec2 리눅스 20.04
+ 프론트: Vue3 CLI - 주소: https://localhost:8081
+ 백엔드: Intellij, Spring Boot, maven - 주소: https://localhost:8443

### 1. AWS ec2 인스턴스 설치하기 
이건 정말 인터넷만 쳐봐도 너무너무 잘되어 있기 때문에 간략하게 넘어가겠다.