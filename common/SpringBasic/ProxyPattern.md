## 프록시 패턴

### 1. 개요

프록시를 한글로 직역하면 대리인, 대변인이라는 뜻을 가진 단어이다.
대리자/대변인은 다른 누군가를 대신해서 그 역할을 수행하는 존재를 말한다.
프록시 패턴의 예시로 수표를 들 수 있다. 현금 대신 사용할 수 있으며 필요할 때 현금과 동일하게 사용 가능하다.

프록시 패턴의 경우 실제 서비스 객체가 가진 메서드와 같은 이름의 메서드를 사용하는데, 이를 위해 인터페이스를 사용한다.
프록시 패턴이 적용되기 전 UML을 보자.

![](https://images.velog.io/images/dailyzett/post/af7d436b-154d-4736-9b5f-59fc40765bbb/003.png)

단순히 ClientWithNoProxy가 Service 객체의 runSomething() 메서드를 직접 호출한다.
이것을 프록시 패턴을 적용하면 다음과 같다.

![](https://images.velog.io/images/dailyzett/post/e6395bde-9036-4bc6-92ae-593e56ed7c88/005.png)

인터페이스 생성 후 해당 인터페이스를 구현하는 Proxy 객체를 만들고 이 프록시 객체를 통해 실제 서비스를 호출한다.

#### 1.1 프록시 객체를 통해 서비스를 호출하는 이유

직접 서비스를 호출하는 것이 더 편해보이는데 굳이 중간 다리인 프록시 객체를 생성하는 이유가 뭘까?
프록시 패턴은 실제 서비스 메서드의 리턴값에 가감하는 것을 목적으로 하지 않고 **제어의 흐름을 변경하거나
다른 로직을 수행하기 위해 사용**한다. 예를 들어, 회원 정보 조회 서비스에 접근하기 위한 보안 절차가 필요한데,
보안 절차 전부를 회원 정보 조회 로직에 전부 우겨넣는 것은 객체 지향적인 코드라고 볼 수 없다.
단일 책임 원칙에 따르면 회원 정보 조회 서비스는 오로지 회원 정보 조회와 관련된 로직만 담겨 있어야 한다.
따라서 클라이언트에서 어떤 서비스에 접근할 때, 중요 서비스 객체의 복잡성을 줄이기 위해 래퍼 객체를 생성해야할 때
프록시 패턴을 사용한다.

### 2. 예제

```java
public interface IService {
    String runSomething();
}
```

```java
public class Service implements IService{
    @Override
    public String runSomething() {
        return "서비스를 실행합니다";
    }
}
```

IService 인터페이스를 구현한 Service 클래스를 만든다.
여기까지는 인터페이스를 구현한 일반적인 서비스 클래스이다.
하지만 프록시 패턴을 적용한다면 IService를 구현하는 Proxy 클래스를 생성해야한다.

```java
public class Proxy implements IService{
    IService service1;

    @Override
    public String runSomething() {
        System.out.println("호출에 대한 흐름 제어가 주목적, 반환 결과를 그대로 전달");

        service1 = new Service();
        return service1.runSomething();
    }
}
```
Proxy 클래스의 runSomething() 메서드에서 service 객체 메서드의 리턴값은 그대로 전달되지만,
그 위의 System.out.println 코드를 한 줄 더 실행하는 것을 볼 수 있다. 여기서는 단순한 출력문이지만
실제로 if문을 사용해 어떤 조건을 만족하지 못하면 강제로 Exception을 던져 인증되지 않은 사용자라면 접근을
거부하도록 만들 수 있다.
그리고 프록시 클래스를 사용하는 메인 메서드는 다음과 같이 작성한다.

```java
public class Main {
    public static void main(String[] args) {
        IService proxy = new Proxy();
        System.out.println(proxy.runSomething());
    }
}
```

```java
출력결과 :
호출에 대한 흐름 제어가 주목적, 반환 결과를 그대로 전달
서비스를 실행합니다
```

### 3. 특징

프록시 패턴의 특징은 다음과 같다.
- 프록시는 실제 서비스와 같은 이름의 메서드를 구현한다. 이때 인터페이스를 사용한다.
- 프록시는 실제 서비스에 대한 참조 변수를 갖는다(Composition)
- 프록시는 실제 서비스의 메서드 호출 전후에 별도의 로직을 수행할 수도 있다.

프록시 패턴을 한 문장으로 정리해보자

> 제어 흐름을 조정하기 위한 목적으로 중간에 대리자(프록시)를 두는 패턴

### 4. 관련 패턴과의 차이

어댑터 패턴, 프록시 패턴, 데코레이터 패턴은 비슷한 것 같지만 다른점이 있다.
일단 어댑터 패턴은 어댑터를 제공할 때 목적과 다른 인터페이스를 제공하는 반면, 프록시는 원래 객체와 동일한 인터페이스를 제공한다.
데코레이터도 프록시 패턴과 이런 부분은 동일하지만 프록시가 실제 서비스 메서드의 반환값을 그대로 리턴해주는 반면
데코레이터 패턴은 이 리턴값에 추가 코드(로직)을 덧입힌다.

