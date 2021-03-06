## HTTP

### 1. 개요

HTTP는 **H**yper**T**ext **T**ransfer **P**rotocol의 약자로, 처음은 HTML 파일을 전송하는 프로토콜로 시작했지만
현재는 거의 모든 데이터를 HTTP 메시지에 담아 전송하고 있다.

- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML(API)
- 서버 간 데이터를 주고받을 때도 HTTP 사용

### 2. 버전

HTTP도 기술이 발전될수록 버전이 업데이트 되었다. 그 중 가장 중요한 것은 **HTTP/1.1**이다.
HTTP/2 와 HTTP/3 의 경우 HTTP/1.1 에서 성능만 개선한 부분이고 필요한 기능은 전부 HTTP/1.1에 담겨있기 때문이다.
구글 크롬에서 F12 버튼을 눌러 사용하고 있는 HTTP 프로토콜 버전을 볼 수 있다.

![](https://media.vlpt.us/images/dailyzett/post/0a3c249a-6784-49ca-b601-a93744083913/image.png)

표의 프로토콜에서 h3은 HTTP/3 을 뜻하고 h2는 HTTP/2 를 뜻한다.
전체 웹을 봤을 때는 HTTP/1.1을 주로 사용하긴 하지만, 시간에 따라 HTTP/2, HTTP/3 도 점점 증가할 것이다.
앞서 설명했다시피, HTTP/2 까지는 TCP 통신 방식이지만 HTTP/3 부터는 UDP 통신 방식으로 변경되었다.

### 3. HTTP 구조와 특성

HTTP는 클라이언트와 서버 구조로 나뉘어져 있다.
이러한 구조에서 얻을 수 있는 이점은 클라이언트는 사용자의 UI/UX나 사용성에만 집중하고 서버는 비즈니스 로직과 데이터 조작에만 집중하면 된다는 점이다.

#### 3.1 stateless vs stateful

stateless 는 상태유지를 하지 않겠다라는 뜻이고 stateful 은 상태를 유지한다는 뜻이다.
그리고 HTTP는 stateless를 지향한다. 다음은 stateful과 stateless 의 예시다.

**stateful**
```text
클라이언트 : 토비의 스프링 책은 얼마인가요?
서버 : 36000원입니다.

클라이언트 : 그거 두 개 주세요.
서버 : 72000원입니다.

클라이언트 : 네. 결제는 신용카드로 할게요
서버 : 네 알겠습니다.
```

**stateless**

```text
클라이언트 : 토비의 스프링 책은 얼마인가요?
서버 : 36000원입니다.

클라이언트 : 토비의 스프링 책 두 개 주세요.
서버 : 72000원입니다.

클라이언트 : 토비의 스프링 책 두 개, 72000원은 신용카드로 결제할게요.
서버 : 네 알겠습니다.
```

stateful은 상태 유지를 하고 있기 때문에 그 전의 데이터가 무엇인지 기억하고 있다. 하지만 stateless 는 클라이언트가
요청을 할 때마다 필수 정보들을 항상 보낸다. 얼핏 보면 stateful이 효율적인 것 같지만 그렇지 않다.
stateful이면서 서버가 다른 서버로 교체됐을 때 상태 유지 데이터를 다른 서버에게 그대로 보내줘야한다.
클라이언트와 서버가 열심히 요청과 응답을 하다가 서버가 고장나서 다른 서버에서 응답을 받아야하는 경우, 다른 서버는 이전 서버에게서 데이터를 반드시 인계받아야 한다.

하지만 stateless 의 경우 그럴 필요가 없다.
어떤 동작을 수행할 때 필수 정보들을 같이 보내기 때문에 서버가 교체돼도 데이터를 옮길 필요 없이 바로 해당 작업의 처리가 가능하다.
이런 HTTP의 stateless 특성 덕분에 서버의 확장도 매우 쉬워진다. 다만 이런 stateless에도 몇 가지 한계가 있다.

먼저 로그인과 같이 사용자 정보가 무조건 유지되어야 할 때 stateless 로만 설계하기는 불가능하다. 
그래서 필요할 때만 상태유지를 할 수 있도록 클라이언트 측에서 사용되는 **쿠키**와 서버 측에 사용되는 **세션**을 적절히 조합해 사용한다. 
또 한가지 단점은 stateless는 필수 데이터들을 항상 전송해야하기 때문에, stateful 보다 전송 데이터 양이 많아질 수 밖에 없다.

#### 3.2 connectionless

HTTP는 비연결성(connectionless)를 지향한다.

![](https://media.vlpt.us/images/dailyzett/post/2debfe3e-15f3-45df-8363-e5a8e7cb0b2a/image.png)

현재 서버가 다음과 같은 상황이라고 가정한다.

- 클라이언트1이 서버에 요청을 하고 서버가 응답을 한 상태이다.
- 그리고 서버는 클라이언트1의 **연결 상태를 유지**한다.
- 클라이언트2가 서버에 요청을 한다.
- 서버는 클라이언트2의 연결 상태를 또 유지한다.
- 클라이언트3이 서버에 요청을 한다..
- ...

연결된 상태를 계속 유지할 경우 서버 리소스를 쓸데없이 낭비하게 된다.
서버 리소스를 효율적으로 관리하기 위해서, 클라이언트1에게 맞는 응답이 끝나면 바로 연결을 끊는 것이 현명한 방법이다.
이것이 HTTP가 비연결성을 지향하는 이유이다.

다만 비연결성에도 단점이 있다.
클라이언트1의 연결이 끊긴 후 재연결 요청이 들어왔을 때 처음부터 다시 시작해야한다.
처음부터 다시 시작한다는 말은 TCP의 3-handshake 부터 HTML/CSS/JavaScript 및 각종 정적 파일을 다시 새로 불러와야된다는 뜻이다.
그래서 HTTP는 이것을 Persistent Connection(지속 연결) 방식으로 해결한다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/HTTP_persistent_connection.svg/1280px-HTTP_persistent_connection.svg.png)

사진을 보면 Persistent connection을 적용했을 때 클라이언트의 open과 close는 한 번이지만 Multiple Connections는
요청때마다 open과 close가 반복된다. 당연히 후자가 시간이 오래걸릴 수 밖에 없다.
Persistent connection 을 이용해서 하나의 TCP 연결을 사용해 복수의 HTTP 요청/응답을 주고 받을 수 있다.
HTTP/2 프로토콜은 동일한 개념을 사용하며 더 나아가 하나의 연결 **복수의 동시 요청/응답을 다중화**하는 것이 가능하다.

### 4. HTTP 메시지

![](https://media.vlpt.us/images/dailyzett/post/56039a2e-c3ff-4532-8104-61f721cd05b2/image.png)

HTTP는 클라이언트가 보내는 요청이냐 또는 서버가 보내는 응답이냐에 따라 보내는 메시지 내용이 달라진다.

HTTP 메시지의 시작 라인(빨간색 박스)은 다음과 같이 구성된다.
- HTTP 메서드
- 요청 대상
- HTTP Version

그리고 응답 메시지는 다음과 같이 구성된다.
- HTTP 버전
- HTTP 상태 코드
  - 200: 성공
  - 400: 클라이언트 요청 오류
  - 500: 서버 내부 오류
- 이유 문구 : 사람이 이해할 수 있는 짦은 설명글

HTTP 헤더(노란색 박스)에는 HTTP 전송에 필요한 모든 부가정보가 담긴다. 필요시 임의의 헤더도 추가 가능하다.

파란색 박스는 HTTP 메시지 바디인데, 여기에 실제 전송할 데이터가 모두 담긴다.
실제 전송할 데이터라함은 HTML 문서, 이미지, 영상, XML 등등 byte로 표현할 수 있는 모든 데이터를 뜻한다.

### 5. 참고 자료

> (인프런) 모든 개발자를 위한 HTTP 웹 기본 지식

> https://ko.wikipedia.org/wiki/HTTP/2
