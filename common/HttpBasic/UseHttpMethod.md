## HTTP 메서드 활용

### 1. 클라이언트에서 서버로 데이터 전송

- 정적 데이터 전송 : 이미지, 정적 테스트 문서
- 동적 데이터 조회 : 주로 검색, 게시판 목록에서 정렬 필터(검색어)
- HTML Form 태그를 이용한 데이터 전송 : 회원 가입, 상품 주문, 데이터 변경
- HTTP API 를 통한 데이터 전송 : 
  - 회원 가입, 상품 주문, 데이터 변경
  - 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

### 2. 회원 관리 시스템 API 설계

member 라는 회원이 있을 때 HTTP 메서드를 통해 다음과 같이 설계할 수 있다.

- 전체 회원 목록 : GET /members
- 특정 회원 조회 : GET /members/{id}
- 회원 등록 : POST /members
- 회원 정보 수정 : PUT, PATCH, POST /members/{id}
- 회원 정보 삭제 : DELETE members/{id}

#### 2.1 리소스를 등록할 때 PUT, PATCH, POST 의 차이

먼저 PATCH는 회원 정보의 일부만 변경해야할 때 사용할 수 있다. 
반면 게시글 수정같은 경우 게시글 전체를 대체하는게 좋으므로 PUT과 POST를 사용하면 된다.
그런데 POST도 요청에 따라 데이터를 수정할 수 있고 PUT도 수정할 수 있다.
POST와 PUT의 차이점은 리소스 URI를 서버가 관리하느냐 클라이언트가 관리하느냐에 있다.

##### 2.1.1 POST - 컬렉션

먼저 POST 요청을 보낼 때, 클라이언트는 전송할 리소스의 URI를 모른다.
이 URI는 데이터를 처리할 때 서버가 알아서 부여한다.
서버가 관리하는 리소스 디렉토리를 **컬렉션**이라고 한다. 컬렉션의 특징은 다음과 같다.

- 서버가 관리하는 리소스 디렉토리
- 서버가 리소스의 URI를 생성하고 관리

##### 2.1.2 PUT - 스토어

파일 등록을 설계하기 위해 HTTP API를 **PUT /files/{filename}** 으로 설계할 수 있다.
POST와의 차이점은 PUT 메서드는 리소스 URI를 클라이언트에서 알아야 된다는 점이다.
만약에 PUT 메서드를 이용해 starts.jpg 라는 파일을 올리고 싶을 때 PUT /files/stars.jpg 처럼 클라이언트가 직접 리소스 URI를 지정해야한다.
그리고 클라이언트가 관리하는 리소스 디렉토리를 **스토어**라고 한다.

- 클라이언트가 관리하는 리소스 저장소
- 클라이언트가 리소스 URI를 알고 관리

### 3. HTML FORM 사용

HTML Form 은 단순하지만 GET과 POST 메서드만 지원한다는 단점이 있다.
물론 AJAX 같은 기술을 사용해서 HTTP 메서드 API를 직접 구현하면 이 문제는 해결되지만, HTML Form 만으로 어떻게 HTTP 메서드를 설계해야하는지 알아보자.

- 회원 목록 : GET /members
- 회원 등록 폼 : GET /members/new
- 회원 등록 : POST /members/new
- 회원 조회 : GET /members/{id}
- 회원 수정 폼 : GET /members/{id}/edit
- 회원 수정 : POST /members/{id}/edit
- 회원 삭제 : POST /members/{id}/delete

설계된 URI를 잘 보면 잘 설계되었다고 말할 수 없다.
일단 동작을 설명하는 new, edit, delete가 URI에 들어갔기 때문이다.
하지만 이런 경우는 리소스는 명사만 사용해야 한다는 규칙을 깨뜨릴 수밖에 없다.
GET, POST만 지원하기 때문에 설계에 제약이 생기기 때문이다.
이런 제약을 해결하기 위해 동사로 된 리소스 경로를 사용하는데 이것을 **컨트롤 URI**라고 한다.
컨트롤 URI는 HTTP 메서드로 해결하기 애매한 경우에 사용한다.




