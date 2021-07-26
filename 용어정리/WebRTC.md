# WebRTC란
### [유튜브 What is WebRTC 참고](https://www.youtube.com/watch?v=0XceZw0Ktn0)


WebRTC란 브라우저에서 Real Time Communication을 지원해주는 오픈 프레임워크이다.
WebRTC는 높은 퀄리티의 웹상의 통신을 위한 기초적인 소스코드를 제공해준다. 예를 들어 비디오나 채팅 앱에서 사용하는 네트워크, 오디오, 비디오 기능 등..


브라우저에서 실행되는 WebRTC는 `JavaScript APIs`로 접근 가능하다. 그래서 개발자들이 쉽게 RTC를 가지고 웹을 만들 수 있 API는 국제적 인터넷 프로토콜 협회인 IETF

WebRTC API는 W3C에서 표준화되었으며 프로토콜 레벨에선 국제 인터넷 표준화 기구인 IETF에서 표준화 되었다. 그리고 구글, 모질라, 오페라 등 대기업에서 지원하고 있다.


+ getUserMedia
```javascript
navigator.mediaDevices.getUserMedia(streamCosntrains).then(function(stream){
    localStream = stream;
    localVideo.srcObject = stream;
})
```
유저가 미디아 디바이스에 접근하는 코드. 유저가 프롬프트 창에서 권한허가하는 창이 보이게 됨.


+ RTCPeerConnection
유저가 로컬 미디아 스트림을 만들면 RTCPeerConnection 객체를 만들게 됨. 이걸 보내서 peer to peer로 다른 브라우저(사람들)과 통신하게 됨
![image](https://user-images.githubusercontent.com/24693833/126992938-8cb6d361-0ab7-4630-ba2d-d23a1ef785e4.png)


+ Data Channel
![image](https://user-images.githubusercontent.com/24693833/126993019-33366c19-d48a-4888-b6b9-8b3443f167c6.png)
WebRTC는 실시간 통신할 수 있는 영상정보, 오디오정보, 채팅정보 등 모든 것을 보낼 수 있음


## Why WebRTC
1. WebRTC enables communication in the browser
2. Includes NAT and firewall traversal technology using STUN, ICE, TURN, RTP-over-TCP and support proxies
3. Most modern browsers today support it


<br>

### WebSockets vs WebRTC
![image](https://user-images.githubusercontent.com/24693833/127002343-8947d6cc-e301-46d7-8a32-69df254ac2dc.png)
웹소켓은 오로지 클라이언트-서버와 통신만 가능함. 양방향 통신이 가능하지만 유튜브처럼 peer-to-peer 영상사이트로 만들기엔 서버 부담이 너무 크다. 

![image](https://user-images.githubusercontent.com/24693833/127002438-e0e9d22f-b99f-4300-8cbe-630c66357232.png)
WebRTC는 클라이언트끼리 바로 통신이 가능함. 서버에서 데이터를 받아오는 데 필요한 latency 없어서 빠름. 화상채팅 구현하기에 딱!!

ref) https://www.youtube.com/watch?v=2Z2PDsqgJP8


### Signaling
클라이언트끼리 서로 통신할 수 있는 상태라는 거승ㄹ 아는 방법. Client A와 Client B가 있을 때 둘이 통신하고 싶다면 서버를 매개로 서로 정보를 주고받고 연결함. 그 후에 클라이언트끼리 서버 없이 통신.
그러나 WebRTC는 UDP로 통신하므로 데이터 손실 가능성이 높다. 그래서 시그널링을 할 때 WebSockets가 필요함


