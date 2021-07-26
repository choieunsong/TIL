# WebSocket 함수를 알아보자
### reference
+ https://nowonbun.tistory.com/285
+ [Mozila](https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications)
-----

WebSocket 프로토콜 요청은 `ws//~`나 `wws://~`로 시작한다.  
#### 서버측 코드(java)
```
//웹소켓의 호스트 주소 설정
@ServerEndpoint("/websocket")
public class Websocket{
  //웹소켓으로 메시지가 오면 요청되는 함수
   @OnOpen
   public void handleOpen(){
    System.out.println("client is now connected...");
   }
   //웹소켓으로 메시지가 오면 요청되는 함수
   @OnMessage
   public String handleMessage(String message){
     //메시지 내용을 콘솔에 출력한다
      System.out.println("received from client: "+message);
   }
   //웹소켓과 브라우저가 접속이 끊기면 요청되는 함수 
   @OnClose
   public void handleClose(){
      //콘솔에 접속 끊김 로그 출력 
      System.out.println("client is now disconnected...");
   }
   //웹소켓과 브라우저 간에 통신 에러가 발생하면 요청되ㅣ는 함수
   @OnError
   public void handleError(Throwable t){
      //콘솔에 에러를 표시한다.
      t.printStackTrace();
   }
}
```
웹소켓은 @OnOpen, @OnMessage, @OnClose, @OnError 네가지 어노테이션으로 브라우저와 통신 이벤트를 관리함

#### 클라이언트측 코드(javascript)
```javascript
var ws = new WebSocket("ws://localhost:8080/WebSocketSessionShare/websocket");
var messageTextArea = document.getElementById("messageTextArea");

ws.onopen = function(message){
  messageTextArea.value += "Server connected...\n";  
}

ws.onclose = function(message){
  messageTextArea.value += "Server Disconnected...\n";
}

ws.onerror = functino(message){
  messageTextArea.value += "error...\n";
}

ws.onmessage = function(message){
  messageTextArea.value += "Recieve From Server => " + message.data + "\n";
}
//send 버튼을 누르면 세션을 가져온다
function sendMessage(){
 messageTextArea.value += "Get Http Session";
 ws.send("get");
}
function disconnect(){
  ws.close();
}
```
클라이언트 리스너 함수로 onopen, onmessage, onclose, onerror가 있다. 거의 서버와 비슷함

<br>


**json으로 데이터 전송하기**    
채팅 프로그램에서 서버와 JSON으로 캡슐화된 패킷 데이터를 주고받는 프로토콜 구현할 때
```javascript
// Send text to all users through the server
function sendText() {
  // Construct a msg object containing the data the server needs to process the message from the chat client.
  var msg = {
    type: "message",
    text: document.getElementById("text").value,
    id:   clientID,
    date: Date.now()
  };

  // Send the msg object as a JSON-formatted string.
  exampleSocket.send(JSON.stringify(msg));

  // Blank the text input element, ready to receive the next line of text from the user.
  document.getElementById("text").value = "";
}
```

#### 서버로부터 데이터 수신하기
```javascript
exampleSocket.onmessage = function (event) {
  console.log(event.data);
}
```
parameter로 message 이벤트가 onmessage 함수로 전달됨. 데이터를 수신하는 코드

#### JSON 오브젝트 받아서 처리하기
클라이언트가 json 데이터를 받는 경우 다음과 같은 상황이 있다.
로그인 핸드쉐이크, 메시지 텍스트, 유저 목록 업데이트. 보통 json에 type 값을 넣어서 switch로 
```javascript
exampleSocket.onmessage = function(event) {
  var f = document.getElementById("chatbox").contentDocument;
  var text = "";
  var msg = JSON.parse(event.data);
  var time = new Date(msg.date);
  var timeStr = time.toLocaleTimeString();

  switch(msg.type) {
    case "id":
      clientID = msg.id;
      setUsername();
      break;
    case "username":
      text = "<b>User <em>" + msg.name + "</em> signed in at " + timeStr + "</b><br>";
      break;
    case "message":
      text = "(" + timeStr + ") <b>" + msg.name + "</b>: " + msg.text + "<br>";
      break;
    case "rejectusername":
      text = "<b>Your username has been set to <em>" + msg.name + "</em> because the name you chose is in use.</b><br>"
      break;
    case "userlist":
      var ul = "";
      for (i=0; i < msg.users.length; i++) {
        ul += msg.users[i] + "<br>";
      }
      document.getElementById("userlistbox").innerHTML = ul;
      break;
  }

  if (text.length) {
    f.write(text);
    document.getElementById("chatbox").contentWindow.scrollByPages(1);
  }
};
```
