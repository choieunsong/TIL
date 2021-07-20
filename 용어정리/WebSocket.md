# WebSocket이란
[우아한 테크코스 - 코일읠 Web Socket](https://www.youtube.com/watch?v=MPQHvwPxDUw&t=656s)

### 웹 소켓이란?
+ html5에서 지원하는 두 프로그램 간의 메시지를 교환하기 위한 통신 방법 중 하나
+ W3C와 IETF에 의해 자리잡은 표준 프로토콜 중 하나이다. 

### 웹 소켓의 특징
1. 양방향 통신(Full-Duplex)
   - 데이터 송수신을 동시에 처리할 수 있는 통신 방법
   - 클라이언트와 서버가 서로에게 원할 때 데이터를 주고 받을 수 있다.
   - 통상적인 Http 통신은 Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 통신  
2. 실시간 네트워킹(Real time networking)  
   - 웹 환경에서 연속된 **데이터를 빠르게 노출**
   - ex) 채팅, 주식, 비디오 데이터
   - 여러 단말기에 빠르게 데이터를 교환  
3. 웹 소켓 이전의 비슷한 기술  
   - `Polling`
     -  서버로 일정 주기로 요청을 송신
     - real-time 통신에서는 언제 통신이 발생할지 예측이 불가능. 불필요한 request와 connection을 생성
   - `Long Polling`
     - Polling의 단점을 보완하고자 만들어짐
     - 서버에 요청 보내고 이벤트가 생겨 응답을 받을 때 까지 연결 종료 X
     - 많은 양의 메시지가 쏟아질 경우 Polling과 같다
   - `Streaming`
     - 서버에 요청을 보내고 끊기지 않은 연결 상태에서 끊임없이 데이터를 수신
     - 클라이언트에서 서버로의 데이터  송신이 어렵다   

![image](https://user-images.githubusercontent.com/24693833/126273453-7702d123-d7bd-41ef-9d0e-6e6026ffe57f.png)

> 결과적으로 이 모든 방법이 HTTP를 통해 통신하기 때문에 Request, Response 둘 다 **Header가 불필요하게 큼**

 
### 웹 소켓의 동작 방법 - 핸드 쉐이킹
 