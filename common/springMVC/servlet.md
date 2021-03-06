# 1.2 서블릿

## HTML Form 데이터 전송

![img_4.png](image_1/img_4.png)

Form 을 통해 POST 요청을 보냈다고 가정한다.
이 때 웹 서버를 개발자가 전부 구현해야 된다면 어떻게 될까?

1. TCP/IP 연결을 대기할 수 있도록 코드를 작성한다.
2. HTTP 요청 메시지를 파싱해서 읽는 코드를 작성한다.
3. HTTP 메시지가 어떤 메서드인지 파악하고, /save URL을 인식한다.
4. Content-Type 을 분석 후, username과 age 데이터를 사용할 수 있게 파싱한다.
5. 저장 프로세스를 실행한다.
6. **비즈니스 로직을 실행한다.**
   1. **DB에 저장을 요청한다.**
7. HTTP 응답 메시지 생성을 시작한다.
   1. HTTP 시작 라인 생성
   2. Header 생성
   3. 메시지 바디에 HTML 생성에서 입력
8. TCP/IP에 응답을 전달하고 소켓을 닫는다.


사실 개발자가 가장 관심있는 부분은 6번과 6-1번이다.
그런데 그 전 단계와 후 단계가 너무 많다. 웹을 구현해야 하는 모든 개발자들이 이것을 하나씩 다 구현한다는 것은 너무 비효율적이다.
이런 문제점을 해결하기 위해 **서블릿**이 탄생하게 된다.

서블릿은 6번과 6-1번을 제외한 나머지 모든 작업들을 지원해준다. 정확히는 서블릿을 지원하는 WAS가 전처리 작업을 전부 제공해준다.

## 서블릿

서블릿은 아래와 같이 생겼다.

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet{
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response){
        //TODO
    }
}
```

`urlPattenrs`의 URL이 호출되면 서블릿 코드가 실행된다.
`service` 메서드를 보면 두 개의 매개변수가 있는데 이 매개변수를 통해 HTTP 요청 정보와 응답 정보를 편리하게 사용 가능하다.

URL 요청과 서블릿이 하는 작업들을 그림으로 표현하면 아래와 같다.

![img_5.png](image_1/img_5.png)

1. 요청 메시지를 기반으로 request/response 객체를 새로 만든다.
2. 서블릿 컨테이너에서 `helloServet` 을 실행한다.
3. `helloServlet` 작업이 끝나면 response 객체로 가서 응답 메시지를 작성한다.
4. 마지막으로 웹 브라우저에 응답 메시지를 전달한다.

## 서블릿 컨테이너
 
`HelloServlet` 클래스를 생성했을 때 서블릿 객체를 WAS의 서블릿 컨테이너란 곳에서 자동으로 생성해준다.
그리고 위 그림처럼 호출도 자동으로 해주고 WAS가 시작될 때 시작되고 종료될 때 자동으로 꺼지는 생명 주기도 가지고 있다.

서블릿 컨테이너를 한 문장으로 정의하면 어떻게 될까?
**웹을 개발할 때 Tomcat처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 한다.**

#### 서블릿 컨테이너의 특징

- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기를 관리한다.
- 서블릿 객체는 **싱글톤**으로 관리한다.
  - 스프링 싱글톤 빈과 마찬가지로, 웹 브라우저의 요청이 올 때마다 계속 객체를 생성하는 것은 비효율적이다.
  그래서 서블릿 또한 최초 로딩 시점에 미리 객체를 만들어두고 이를 재활용한다.
  - 그렇기 때문에 서블릿 객체에 공유 변수를 사용하는 것은 상당한 주의를 기울여야 한다.
  모든 요청이 공유 변수를 사용할 수 있기 때문에 요청마다 변수가 업데이트 되어 예상치 못한 결과를 발생시킬 수 있기 때문이다.

- JSP도 서블릿으로 변환 되어서 사용된다.
- 동시 요청을 위한 멀티 쓰레드 처리를 지원한다.

여러 클라이언트에서 동시 요청이와도 서블릿이 정상적으로 처리할 수 있는 이유는 서블릿 객체가 동시 요청을 위한 멀티 쓰레드 처리를 지원하기 때문이다.
그래서 개발자는 멀티 쓰레드같은 복잡한 부분을 크게 신경쓰지 않아도 된다는 장점이 있다.


## 출처

> [인프런 강의 - 스프링 MVC 1편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)