# WebSocket이란
[우아한 테크코스 - 코일의 Web Socket 참고](https://www.youtube.com/watch?v=MPQHvwPxDUw&t=656s)

### 웹 소켓이란?
+ html5에서 지원하는 두 프로그램 간의 메시지를 교환하기 위한 통신 방법 중 하나
+ W3C와 IETF에 의해 자리잡은 표준 프로토콜 중 하나이다. 
+ 웹소켓은 클라이언트가 서버에게 요청하지 않아도 서버에서 메시지를 받을 수 있는 통신이다.

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
     - 서버로 일정 주기로 요청을 송신
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

<br> 

### 웹 소켓 동작 방법 - 핸드 쉐이킹

**요청방법**    
```
Client(http(80) or https(443)) -> Server(http(80) or https(443))
```
```html
GET /chat HTTP/1.1        
Host: server.example.com  
Ugprade: websocket       
Connection: Upgrade     
Sec-WebSocket-Key: dGhlhLNgiefnLnf7sdf==   
Origin: http://example.com     
Sec-WebSocket-Protocol: chat. superchat   
Sec-WebSocket-Version: 13
```
+ GET: 반드시 GET 메서드를 사용해야 함. HTTP 버전은 반드시 1.1 이상
+ Host: 웹서버의 주소
+ Upgrade: 현재 클라이언트, 서버, 전송 프로토콜 연결에서 다른 프로토콜로 업그레이드 또는 변경하기 위한 규칙. 
+ Connection: Upgrade 헤더 필드가 명시되었을 경우, 송신자는 반드시 Upgrade 옵션을 지정한 Connection 헤더 필드도 전송
+ Sec-WebSocket-Key: 길이가 16바이트인 임의로 선택된 숫자를 base64로 인코딩 한 값
+ Origin: 클라이언트의 주소
+ Sec-WebSocket-Protocol: 
    + 클라이언트가 요청하는 여러 서브프로토콜을 의미
    + 공백문자로 구분되며 순서에 따라 우선권이 부여
    + 서버에서 여러 프로토콜 혹은 프로토콜 버전을 나눠서 서비스할 경우 필요한 정보



**응답방법**   
```
Client(http(80) or https(443)) <- Server(http(80) or https(443))   
```
```html
HTTP/1.1 101 Switching Protocols
Uprade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: sdFgwsekfSDFlkfEDFklmsdf+sd
```
+ HTTP: 101 Switcing Protocols가 Response로 오면 웹소켓이 연결
+ Sec-WebSocket-Accept
  + 클라이언트로부터 받은 Sec-WebSocket-Key를 사용하여 계산된 값
  + 요놈이 클라이언트에서 계산된 값과 일치하지 않으면 연결 수립 X

<br>

### 웹 소켓 동작 방법 - 데이터 전송
핸드쉐이킹이 완려된 후에 데이터를 전송할 때는 프로토콜이 ws, wws로 바뀜. wws는 데이터 보안을 위해서 SSL을 적용한 프로토콜
```
                            WebSocket
Client(ws(80) or wws(443))  -------->    Server(ws(80) or wws(443))
                            WebSocket
Client(ws(80) or wws(443))  <--------    Server(ws(80) or wws(443))
```
데이터를 주고받을 때 Message라는 단위를 사용한다.
+ Message: 여러 Frame이 모여서 구성하는 하나의 논리적 메시지 단위
+ Frame: communication에서 가장 작은 단위의 데이터, `작은 헤더 + payload`로 구성   
+ 웹소켓 통신에 사용되는 데이터는 `UTF8 인코딩`   
  ex) 0x00(보내고 싶은 데이터) 0xff


#### 프레임 헤더 구조
![image](https://user-images.githubusercontent.com/24693833/126870188-7f7ac5aa-f314-45c1-b3b7-f4f14c71b945.png)
+ END: 이 프레임이 전체 메시지의 끝인지 나타내는 플래그
+ Opcode
  + Continue(0x0): 전체 메시지의 일부임을 의미
  + Text(0x1): 포함된 데이터가 UTF-8 텍스트라는 의미
  + Binary(0x2): 포함된 데이터가 이진 데이터라는 의미
  + Close(0x8): Close 핸드쉐이크를 시작한다는 의미
+ Length: 이 프레임에 포함된 데이터의 총 길이를 나타내는 단위



![image](https://user-images.githubusercontent.com/24693833/126870308-7eeaacd3-7769-4009-ab35-c7413f4bc39e.png)

<br>

### 웹 소켓 프로토콜 특징
+ 최초 접속에서만 http 프로토콜 위에서 handshaking을 하기 때문에 http header를 사용한다.
+ 웹소켓을 위한 별도의 포트는 없으며, 기존 포트(http-80, https-443)을 사용
+ 프레임으로 구성된 메시지라는 논리적 단위로 송수신
+ 메시지에 포함될 수 있는 교환 가능한 메시지는 텍스트와 바이너리

<br>

### 웹소켓의 한계 
**Socket.io, SocketJS**  

웹소켓은 HTML5 이후에 만들어졌기 때문에 HTML5 이전에 구현된 서비스에서는 사용 못한다. HTML5 이전에 만들어진 서비스도 웹소켓처럼 통신할 수 있게 나온 것이 Socket.io, SockJS 기술이 출시.  
 + Javascript를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현
 + WebSocket, FlashSocket, AJAX Long Pollingm, AJAX Multi part Streaming, IFRame, JSONP Polling을 하나의 API로 추상화
 + 즉, 브라우저와 웹 서버의 종류의 버전을 파악하여 가장 적합한 기술을 선택하여 사용하는 방식

**STOMP**  
+ WebSocket은 문자열들을 주고 받을 수 있게 해줄 뿐 그 이상의 일은 안해준다.
+ 주고 받은 문자열의 해독은 온전히 어플리케이션에 맡긴다.
+ HTTP는 형식을 정해두었기 때문에 모두가 약속을 따르기만 하면 해석할 수 있다.
+ 하지만 WebSocket은 형식이 정해져 있지 않기 때문에 어플리케이션에서 쉽게 해석하기 힘들다.
+ 때문에 WebSocket방식은 sub-protocols를 사용해서 주고 받는 메시지의 형태를 약속하는 경우가 많다.

| STOMP(Simple Text Oriented Message Protocol)
+ STOMP는 채팅 통신을 하기 위한 형식을 정의
+ HTTP와 유사하게 간단히 정의되어 해석하기 편한 프로토콜
+ 일반적으로 웹소켓 위에서 사용됨

**STOMP의 프레임 구조**  
```
COMMAND
header1: value1
header2: value2

BodyBodyBodyBody^@
```
+ 프레임 기반의 프로토콜이다. 프레임은 명령, 헤더, 바디로 구성된다
+ 자주 사용하는 명령은 CONNECT, SEND, SUBSCRIBE, DISCONNECT등이 있다
+ 헤더와 바디는 빈 라인으로 구분하며, 바디의 끝은 NULL문자로 설정한다.

