## 자바와 객체 지향

### 1. 객체 지향의 4대 특성
- 캡슐화
- 상속
- 추상화
- 다형성

### 1. 추상화

> 추상화란 구체적인 것을 분해해서 관찰자가 관심 있는 특성만 가지고 재조합하는 것이라고 정리할
> 수 있다.

객체 지향의 4대 특성은 클래스로 인해 구현된다. 객체(object)는 **유일무이한 사물**이다.
그리고 클래스는 **같은 특성을 지닌 여러 객체를 포함하는 집합의 개념**이다.

```java
public class Person {
    String name;
    String age;

    public static void main(String[] args) {
        Person kim = new Person();
        Person park = new Person();
    }
}
```

이 코드는 Person 클래스를 이용해서 kim과 park의 객체를 생성했다. 클래스를 이용해서 객체를
만들었다는 것을 강조할 때, 객체라는 표현보다는 클래스의 인스턴스(instance)라는 표현을 쓴다.
보통 클래스가 객체의 설계도라고 하는 표현은 이런 과정에서 나왔다고 보면 된다.

객체 지향 프로그래밍을 할 때 클래스를 가장 먼저 설계한다. 그리고 우리는 객체를 관찰해서 그 객체들 간의 공통된 특성을 포착해서 클래스를 설계한다.
예를 들어 Person(사람) 클래스를 만들었을 때, 이름과 나이같은 속성과 먹다(), 자다() 같은 메서드를 가질 수 있다.

여기서 중요한 것은 클래스를 설계할 때 모든 특성을 담기란 불가능하고, 또 그럴 필요도 없다.
"애플리케이션이 어디에 활용될 것인가?"에 초점을 맞춰서 필요한 특성만 담으면 된다. 즉, 필요한
특성의 **경계**가 필요한데, 이것을 **애플리케이션 경계** 또는 **컨텍스트(Context)** 라고 부른다.

> 내용을 정리하면 추상화는 구체적인 것을 분해해서 **관심 영역**에 있는 특성만 가지고 재조합하는 것이라고
> 설명이 가능하다.

그리고 이러한 추상화의 개념은 모델링의 개념과 동일하다. 모델은 실제 사물을 정확하게 복제하는 것이 아니라
관심 있는 특성만을 추출해서 표현하는 것이기 때문이다. 

그래서 추상화를 요약하면 다음과 같다.
- 객체 지향 프로그래밍에서 **추상화는 모델링**이다.
- 클래스를 설계할 때, **애플리케이션 경계(Context)** 부터 정해야 한다.
- 객체 지향에서 추상화의 결과는 클래스이다.

### 2. 클래스 멤버 vs 객체 멤버

객체들이 여러개 있을 때, 그 객체들 간의 공통 속성이 있다면 따로 빼는 것이 관리하기 편하고 메모리도 절약할 수 있다.
예를 들어, 호랑이라는 클래스를 설계했을 때 기본적으로 호랑이의 다리는 4개이다. 이것을 각 객체별로
속성을 지정하는 것보단, 어느 한 곳에 저장해놓고 공통적으로 사용하는 게 편하다.

이 때 사용되는 것이 static 키워드이다. static 키워드를 속성 앞에 붙이면 그 멤버는 **클래스 멤버**가 된다.
그리고 클래스 멤버는 자바의 T 메모리의 static 영역에 단 하나의 저장 공간을 갖게 된다.
static이 안 붙은 속성을 **객체 멤버**라고 부른다. 즉, static 키워드가 붙었냐 안 붙었냐를 기준으로
클래스 멤버와 객체 멤버를 나눌 수 있다. 이는 메서드도 마찬가지다.

```java
public static void main(String[] args){}
```

Java 애플리케이션에서 가장 먼저 볼 수 있는 main 메서드 앞에도 static 키워드가 붙어 있는데,
main 메서드 역시 클래스 멤버 메서드이기 때문이다. 클래스 멤버 메서드를 정적 메서드라고도 부른다.
main 메서드는 반드시 정적 메서드여야 한다. 그 이유는 T 메모리가 초기화됐을 때 객체 멤버 메서드를 바로 실행할 수
없기 때문이다.

> Note. 스프링을 공부하다보면 다음과 같은 용어를 섞어쓰지만, 모두 같은 뜻이다.
- 정적 멤버, 클래스 멤버, 스태틱 멤버
- 객체 멤버, 오브젝트 멤버, 인스턴스 멤버
- 필드, 속성, 프로퍼티(Property)

#### 2.1 변수의 초기화

다음 코드는 컴파일 에러가 발생한다.
```java
public class Mouse {
    public String name;
    public int age;
    public static int countOfTail = 1;

    public void sing(){
        System.out.println(name + " 찍찍!!!");
    }

    public static void main(String[] args) {
        String localVariable;
        System.out.println(localVariable); // 컴파일 에러

        Mouse m = new Mouse();
        m.sing();
    }
}
```
**localVariable** 은 지역 변수이고 Mouse 클래스 안의 **name** 은 멤버 변수이다.
컴파일 에러가 발생하는 줄을 주석 처리하고 코드를 실행하면 출력값은 다음과 같다.
> null 찍찍!!!

클래스 속성과 객체 속성은 따로 초기화하지 않아도 정수형은 0, 부동소수점형은 0.0,
논리형은 false, 객체는 null로 초기화된다. 하지만 지역 변수는 초기화하지 않으면 그 값을 사용하고자
할 때 컴파일 에러가 발생한다.

> 이유는 멤버 변수는 **공유 변수의 성격**을 가지고 있기 때문이다.

객체 변수는 하나의 객체 안에서 다수의 객체 메서드를 공유한다.
그리고 클래스 변수는 전역 변수로서 프로그램 어디서든 접근 가능하다. 그렇다면 공유 변수는 어디서
초기화를 진행해야 할까? 주로 객체 멤버는 생성자로, static 멤버는 static 실행 영역을 통해 초기화하지만,
공유 변수를 딱히 누군가 초기화해야한다고 규정할 수는 없다. 그래서 공유 변수는 별도로 값을 지정하지 않으면
시스템에서 정한 기본값으로 초기화된다. 반면 지역 변수는 그 지역에서만 사용되는 변수이기 때문에 그 지역에서 초기화하는 것이
논리적으로 맞다.


### 3. 상속: 재사용 + 확장

객체 지향의 상속(Inheritance)은 영어 단어를 한국어로 그대로 번역하면서 오해를 낳는 부분이 있다.
상위 클래스를 부모 클래스, 하위 클래스를 자식 클래스로 부르는 탓에 다음과 같은 오해가 생길 수 있다.

![!img](https://images.velog.io/images/dailyzett/post/47d19a37-6ed3-4e07-8632-43f042184983/Untitled%20Diagram%20(3).jpg)

이 계층 구성도는 상속이라고 말할 수 없다. 동물은 포유류의 부모가 아니고, 조류 또한 마찬가지다.
포유류와 조류는 동물이란 상위 클래스를 조금 더 세분화한 클래스일 뿐이다. 따라서 상속은 단어 그대로의 상속이
아니라 **재사용과 확장** 이라고 말하는 것이 옳다. 따라서 부모 클래스 - 자식 클래스라는 표현보다, 상위 클래스 - 하위 클래스
또는 슈퍼 클래스 - 서브 클래스라고 표기하는 것이 맞다.

예시를 다시 설명하면, 포유류는 동물 클래스의 특성을 **확장**했다고 할 수 있고, 고래는
포유류의 특성을 **확장**했다고 말할 수 있다. 그래서 자바 코드로 클래스를 상속을 할 때, 사용하는 키워드는 **extends**이다.


#### 3.1 상속에서 is-a 관계

상속 관계에서 반드시 만족해야하는 문장이 있다.

> 하위 클래스는 상위 클래스다.

일명 is-a 관계라고 한다.
- 포유류는 동물이다.
- 고래는 포유류이다.
- 참새는 조류이다.

모두 맞는 말인 것 같지만 함정이 있다. "참새는 조류이다" 라는 말에서 참새는 하위 클래스이고
조류는 상위 클래스이다. 즉, is-a 관계에 따라 **하위 클래스 is a 상위 클래스**가 된다.
'a' 에 주목해보자. 한글로 그대로 번역하면 **하위 클래스는 하나의 상위 클래스이다** 로 번역할 수 있다.
하위 클래스는 클래스의 역할을 하는 분류/집단 역할을 한다. 하지만 **하나의 상위 클래스**는 유일무이한
객체이다. 이렇게 되면 삼단 논법에 의해서 **하위 클래스는 하나의 객체이다** 가 된다. 이것은 논리적으로 어긋난다. 클래스 자체는
객체를 분류하기 위한 것이지 객체 그 자체가 아니기 때문이다.

그래서 상속 관계의 더 정확한 표현은 **is a kind of** 이다. 그럼 위의 3개 문장이 다음과 같이 변경된다.

- 포유류는 동물의 한 분류다.
- 고래는 포유류의 한 분류다.
- 참새는 조류의 한 분류다.

훨씬 직관적이고 오해가 생길 여지도 없다.

#### 3.2 다중 상속과 자바

C++은 다중 상속을 지원하지만 자바는 다중 상속을 지원하지 않는다.

![img2](https://images.velog.io/images/dailyzett/post/187eeb4c-53b5-4ccc-a148-17c1cf225527/Untitled%20Diagram%20(4).jpg)

물고기도 수영할 수 있고, 사람도 수영이 가능하다. 여기서 다중 상속을 받은 인어 클래스는 물고기처럼
수영해야할지 사람처럼 수영해야할지 경계가 모호하다. 이와 같은 문제를 다중 상속의 다이아몬드 문제라고 한다.
결국 다중 상속은 득보다는 실이 많았기 때문에 자바는 과감히 다중 상속을 포기했다. 하지만 다중 상속도 편한 부분이 존재하기 때문에
자바는 다중 상속의 득을 취하고 실은 없애는 **인터페이스**라는 개념을 도입했다.

> 상속은 is a kind of 관계이지만 인터페이스는 상속과는 다른 점이 존재한다.

자바의 인터페이스에서 메서드를 만들어놓으면 그것을 구현한 클래스는 해당 메서드를 강제로 구현해야한다.
(Java 8 이후의 default method 제외) 클래스에서 메서드는 행동 혹은 기능을 담당한다. 즉, 인터페이스는
상속의 is a kind of 보다는 **be able to** 관계가 되는 것이 더 정확하다. 실제로 자바에서 이 형식의
인터페이스를 많이 볼 수 있다.

- Serializable : 직렬화 할 수 있는
- Cloneable : 복제할 수 있는
- Comparable : 비교할 수 있는
- Runnable : 실행할 수 있는

### 4. 다형성: 사용편의성

객체 지향에서 다형성이라고 하면 오버로딩과 오버라이딩이라고 할 수 있다. 참고로 오버로딩이 다형성인지 아닌지에 대해서는 이견이
있긴 하다. 어쨌든 상위 클래스와 하위 클래스 사이에서도 다형성을 이야기할 수 있고, 인터페이스와 그것의 구현
클래스에서도 다형성을 이야기할 수 있다.

#### 4.1 오버라이딩 vs 오버로딩

- 오버로딩은 다른 인자 목록으로 다수의 메서드를 중복 정의하는 것을 뜻한다.
- 오버라이딩은 같은 인자 목록으로 상위 클래스의 메서드를 재정의하는 것을 뜻한다.

만약 오버로딩이 없다면 add() 메서드 하나를 추가할 때도 자료형에 따라 각기 다른 메서드를
일일이 제공해야한다. 오버라이딩이 없으면 매번 instanceof 연산자를 사용해서 하위 클래스가 어떤 클래스인지
알아야한다. 그래서 자바는 오버라이딩과 오버로딩 두 개의 개념을 통해 다형성을 제공하고 이 다형성은 개발자가
프로그램을 작성할 때 사용편의성을 제공한다.

### 5. 캡슐화: 정보 은닉

자바에서 캡슐화라고 하면 가장 먼저 떠오르는 것은 접근 제한자이다.

![img3](https://media.vlpt.us/images/gillog/post/30e8db11-f41c-41eb-8737-ae61ebae88d6/99A8EE3E5C243EEA1E.jpg)

하지만 접근 제한자는 그림과 같이 절대 단순하지 않다. 특히 객체 멤버에 대한 접근인가, 정적 멤버에 대한 접근인가에 따라 생각할 것이 많아진다.
protected의 경우 같은 패키지 내의 모든 클래스에서 접근이 가능하다고 하면 생각해 볼 문제가 하나 더 있다.
> first.jar 과 second.jar 파일에서 같은 이름을 가진 packageOne 패키지가 있다고 가정한다.

second.jar 파일 내부의 packageOne 패키지 내 클래스나 객체는 first.jar 파일 내부의
packageOne 패키지 내 클래스나 객체가 가진 **public, default, protected 멤버**에 자유롭게
접근할 수 있다. 이는 보안과 관련해서 많은 위험이 존재한다.

정적 멤버에 대한 접근은 조금 더 생각해 볼 필요가 있다. public 정적 속성인 publicSt의 경우라면
각 위치별 객체 멤버 메서드에서 접근할 수 있는 방법은 세 가지가 있다.

| ClassA                 | ClassA.publicSt | publicSt | this.publicSt |
|:-----------------------|:----------------|:---------|:--------------|
| 같은 패키지 일 때, 상속한 경우     | O               | O        | O             |
| 같은 패키지 일 때, 상속하지 않은 경우 | O               | X        | X             |
| 다른 패키지 일 때, 상속한 경우     | O               | O        | O             |
| 다른 패키지 일 때, 상속하지 않은 경우 | O               | X        | X             |

표를 보면 알겠지만 정적 멤버인 경우 클래스명.정적멤버 형식으로 접근해야 하는 이유는 일관된 형식으로 접근하기 위해서다.
그리고 객체를 생성한 경우 **객체참조변수명.정적멤버** 형태로도 접근할 수도 있다. 하지만 이 방식은 별로 추천하지 않는다.
이유는 물리적 접근에 따른 방식 차이에 있다.

![img5](https://media.vlpt.us/images/dailyzett/post/cecb43c2-45e1-4921-a7c4-2b71eaae3a56/Untitled%20Diagram%20(5).jpg)

(1)은 클래스명.정적멤버 형식이고 (2)는 객체참조변수명.정적멤버 형식이다.
그림만 보더라도, (2)는 값에 도달하기 위해 여러 단계를 거치는 것을 볼 수 있다.
정적 멤버의 경우 마지막 결과는 같은 값이기 때문에 (2)번 방법을 사용할 이유는 없다.

### 6. 참조 변수의 복사

기본 자료형 변수를 복사하는 경우는 Call By Value(값에 의한 호출)이다. 따라서 다음 코드의 두 개의
변수는 서로에게 영향을 주지 않는다.

```java
public class CallByValue {
    public static void main(String[] args) {
        int a = 10;
        int b = a;
        
        b = 20;

        System.out.println(a);
        System.out.println(b);
    }
}

//ouput:
//10
//20
```

하지만 객체 참조 변수를 복사하는 경우는 조금 다르다.

```java
public class Person {
    private int age;

    public static void main(String[] args) {
        Person ref1 = new Person();
        Person ref2 = ref1;
        
        ref1.age = 10;
        ref2.age = 20;

        System.out.println(ref1.age);
        System.out.println(ref2.age);
    }
}

//output:
//20
//20
```

ref1의 주소를 ref2에 대입했기 때문에 현재 ref1이 가르키는 주소와 ref2가 가르키는 주소는
같다. 따라서 ref2.age 값을 20으로 변경하는 순간 ref1이 가르키는 주소에 해당하는 값도 바뀌기 때문에
동일한 출력 결과가 나온다.

```java
ref2.age = 20;
ref1.age = 30;
```

주소값이 같으므로 위와 같이 코드 순서를 바꾸면 결과값은 둘 다 30을 출력할 것이다. 
그리고 이것을 **Call By Reference** 라고 부른다.

Call By Value와 Call By Reference의 이런 특성 때문에 큰 차이가 있는 것 같지만,
본질적으로는 별로 차이가 없다. 전자는 저장하고 있는 값을 값 그 자체로 해석하는 반면,
후자는 저장하고 있는 값을 주소로 해석한다는 차이가 있을 뿐이다.

정리하면 다음과 같다.

- 기본 자료형 변수는 값을 그 자체로 판단한다.
- 참조 자료형 변수는 값을 주소, 즉 포인터로 판단한다.
- 기본 자료형 변수를 복사할 때와 참조 자료형 변수를 복사할 때 일어나는 일은 같다.
즉, 가지고 있는 값을 그대로 복사해서 넘겨 준다. 그게 "값 그 자체"냐 "주소"냐 차이일 뿐이다.

