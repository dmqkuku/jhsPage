---
title: "2023-04-03 Is Exception Bad"
categories: []
tags: [archieve, Java, Exception]
excerpt: "Is Exception Bad"
classess: wide
slug: "Exception2"
published: true
---

**왜 의문을 가졌는가?**
프로그래밍 개념. 언어의 문법, 법칙은 최대한 잘 받아들여야 한다고 생각합니다. (==> 해당 언어를 사용하는데 필수적인 개념이라고 이해하시면 될 것 같습니다.)

이론, 방법론, Guide, Style의 경우 judiciously하게 받아들여야 한다고 생각합니다.

다분히 주관적이지만 예를 들어 설명하자면.

TDD는 법칙이 아니라 이론이나 방법론이기에 무작정 뇌를 빼고 따라하기 보다는, 왜 사용해야 하는지, 어디에 언제 사용해야 하는지, 언제는 사용하면 안되는지 생각을 하고, 조사를 하고 사용해야 합니다.

Exception handling의 경우. Java 문법의 일환이기 때문에 당연하게 받아들일 수 있습니다만.

다음 근거에 따라 "Exception을 사용한다"를 필수적인 개념이 아닌 Guide의 일환으로 받아들이도록 하겠습니다.
1. Exception이 존재하지 않는 Rust와 C의 예를 보아도 알 수 있듯이(제가 알고 있는 Exception이 없는 lang이 Rust와 C뿐입니다. 짧은 식견 이해해주시기 바랍니다.) Exception은 프로그래밍 언어에 있어서 필수 개념이 아닙니다. 물론 이런 식으로 언어에서 "없어도 돌아간다."면서 기능을 빼기 시작하면, "one instruction set computer"처럼 (하나의 Assembly 명령어를 가지는 컴퓨터)만들 수 있다는 점을 자각하고 있습니다. 하지만 Rust에서 Exception을 배제한 것은 분명히 이유가 있을 겁니다.

2. 언어 스펙에 있다고 해서 필수 라고 취급할 수는 없습니다. 악명 높은 modern C++의 경우처럼 말이죠. 오래된 언어인 Java가 만들어질 때부터 존재하던 Exception의 경우(java 1.1 명세 참조. <a href="http://titanium.cs.berkeley.edu/doc/java-langspec-1.0/">The Java Language specification</a>) 추가적으로 다음과 같이 생각할 수 있습니다. 자바 1.1은 1997년 1월에 출시되었습니다. 현재는 2022년입니다. 약 25년 사이에 Exception 사용에 있어서 사람마다 다양한 권고 사항이 있었습니다.


## 논쟁에 대해 살펴보자.

<hr/>
### Exception 반대 측의 논거들
<hr/>

참고 : <a href="https://www.joelonsoftware.com/2003/10/13/13/">JOEL ON SOFTWARE. 13. Exceptions</a href>

#### Exceptions. JOEL.

Java든 C++든 exception을 사용하지 말아야 한다.

따라서
1. 절대 스스로 Exception을 던지지 않는다.
2. 만약 라이브러리에서 Exception을 던질경우 내가 무조건 바로 catch 해버릴 것이다.

저자는 Exception == goto. 라고 생각!

<div class="notice" markdown="1">
```java
function doStupidThings(int a, int b, int c, int d){
    final int dumb = 10;
    try{
        int x = 0;

        x = dumb / a;
        //do something...

        x = dumb / b;
        //do something...

        x = dumb / c;
        //do something...

        x = dumb / d;
        //do something...
    }catch(ArithmeticException ex){
        //...
    }
}
```
각 나누기 부분에서 ArithmeticException 가 발생할 수 있는데. 어디서 터져서 catch로 점프할 지 모른다.!!!
</div>
1. They are invisible in the source code!. 코드 블록의 어느 시점에서 점프할 지 알 수 없다.

2. They create too much possible exit points. 제대로 함수를 짜기 위해서는 모든 가능한 exit path를 머리에 두어야 하는데. exception 때문에 너무 많은 exit path가 생긴다.


저자도 error Code를 사용하면 일견 코드가 지저분해 보일 수 있다고 인정!


그러나 Exception Magic~~ 보다는 낫다!!!


참고 : <a href="https://medium.com/codex/the-error-of-exceptions-3aed074c40dc">The Error of Exceptions</a>

<hr/>
### Exception 찬성 측 의 논거들
<hr/>
참고 : <a href="https://blog.plan99.net/what-s-wrong-with-exceptions-nothing-cee2ed0616">What's wrong with exceptions? Nothing.</a>

#### What's Wrong with Exceptions? Nothing.

저자에 따르면. Go와 Rust를 비롯한 최신 언어들이 Exception을 지원하지 않는 것이 일종의 유행과도 같다. 이런 언어들에서 Exception을 지원하지 않는 "feature"가 일종의 selling point로 기능하는 듯 하다.

저자는 Exception을 모든 언어가 가져야 한다고 주장하며 다음 논지를 펼친다.

Exception의 장점!
1. Code Seperation : Code의 주 로직과 에러 처리 로직의 분리

2. Stack Trace는 언제나 도움이 된다.

3. Failure Recovery. 만약에 "복구는 불가능하거나 하면 안되고, 에러가 발생하면 뻗는게 정상이얌"이러는 프로그래머는 무시하라!


결국 이렇게 좋은 Exception을 사용하지 않는 이유는. 언어 개발자들이 C++출신이고, Exception 구현을 어려워하며, Exception 구현 자체에 낮은 가치를 두고 있기 때문이다!!


저자도 C++에서 Exception이 worse than useless 인 것은 인정!

```cpp
SomeObj foo = new SomeObj();
foo->loadFrom("이 위치에서 exception");
list.push_back(foo);
```
이 코드는 memory leak을 일으킨다. 만약 exception을 삼키고 진행하면, foo는 해제가 되지 않기 때문에...
```cpp
SomeObj foo;
foo->loadFrom("이 위치에서 exception");
list.push_back(foo);
```
이 코드는 스택에 생성이고... exception 터지면 rewind 되면서 foo가 제거된다고 한다.

<div class="notice--primary" markdown="1">
근데 CPP에는 초기화 방법이 장난아니게 많다...

참고 : <a href="https://www.youtube.com/watch?v=7DTlWPgX6zs&t=2706s">CppCon2018: Nicolai Josuttis "The Nightmare of initialization of C++"</a>
</div>

그러나 
1. 객체 복사는 비싸다.
2. 보통 객체 복사는 나름 문제가 있어서 막아둔다. 참고 : <a href="https://google.github.io/styleguide/cppguide.html#Copyable_Movable_Types">구글 스타일 가이드</a>


또한!

ErrorCode 반환시 사용자가 그냥 무시할 수 있다!

에러가 발생한 경우 에러코드를 받는 측에서 바로 해결하는 것 보다. 위쪽으로 propagate시키는 게 나을 수 있다.

<hr/>
<hr/>

## 정리

# Judiciously!!!!!!!!

참고 : <a href="http://www.hanselman.com/blog/good-exception-management-rules-of-thumb">Good Exception management Rules of Thumb</a>

참고 : <a href="https://stackoverflow.com/questions/253314/conventions-for-exceptions-or-error-codes#:~:text=Error%20codes%20can%20be%20ignored,the%20error%20in%20some%20way.">Conventions for exceptions or error codes </a>

Scott Hanselman 이라는 분이 정리한 팁인데. 괜찮은 듯.

1. Exception은 "예외 상황"이어야 한다.  매번 발생하는 일에는 throw exception해서는 안된다.!!

2. 잘 이름 지어진 함수가 이름이 나타내는 일을 하지 못하였을 경우 exception을 던지자!

3. 이미 상황에 잘 맞는 exception이 존재할 경우. 쓸데없이 새로 구현하지 말것

4. 만약 끔찍한 일(거래 transaction failed등)이 일어난다면, keep going할 지 진지하게 고민하라

## 나의 생각 주저리주저리.

<div class="notice" markdown="1">
**C#은 Exception이 한 종류.**
**Exception이 캐치되지 않고, 끝까지 올라올 경우. JVM이 책임지고 프로그램 종료.**
**Java는 임베디드 기기에 탑재되려고 만들어진 언어...**
**C같은 언어에서는 Exception이 존재하지 않는다.**
**웹 서버는 프로그램(서버)가 실행되는 동안. 문제가 발생했을 때. 대처할 사람이 없을 가능성이 크다. 실제로 주말이든 밤이든 이유는 충분. 겪어봄...**
**0으로 나누는 연산 따위를 요즘 시스템에서 수행할 경우. CPU에서 인터럽트/시그널 발생. -> OS에서 캐치하여 프로그램 종료...**
**JVM Exception의 안전성은 OS에서 대신 해주고 있다.**
</div>


생각해 봐야 하는 점...
> 함수는 에러 코드를 던져도 되는가(Old School)? 함수는 무조건 올바른 값을 던지고, 문제가 있을 경우 에러 코드를 던지는 것이 아니라 Exception을 던져야 하는가?


> 함수의 내부를 다른 사용자가 가급적 보지 않고도. 그 함수를 잘 호출해서 사용할 수 있어야 좋은 함수이다.


> 위에서 언급한 Exception을 던지고 바로 받는 구조는 happy path라고 봐야 한다. Exception던지는 곳과 처리하는 곳 사이에 함수가 수십개 있을 수 있고. 비슷한 Exception을 던지는 함수가 call hierachy에서 여러 곳 존재할 수 있다. 즉. 사용자가 Exception을 던지는 함수를 사용하려고 할 경우. 만약 제대로 catch하려면, call hierachy를 죽 흩고. 찾아서 어떠한 Exception을 던지는 것인지 파악해 내야 한다.


Java에서는 C#과 달리(근거는?). Checked/ unchecked 두 종류의 예외가 존재한다. 


여기서 unchecked Exception은 C# 과 동일하다고 봐야 한다.(근거는?) Unchecked Exception은 컴파일러가 따로 검사를 하지 않는다.


Checked Exception은 컴파일러가 검사해 준다. 


예외를 exception.printStackTrace();하기만 하는 것은 의미 없다.


예외는 잡는 사람이. 프로그램의 비정상 작동을 회복시키라는 의미로 만든 듯. === robust (exception safe programming) 라고들 부른다.


예외는 말 그대로. 예상 가능하면 예외가 아니다. 예상 가능한 예외가 있다면. 프로그램 작성시에 대처를 했겠지?


근데 예상이 불가능한데 "어떤"예외가 "어느 시점에" 발생할 지 "예상" 해서 대처하는 코드를 삽입하는게 가능한가?


금융이나. 보안 부분에서는 이 방식이 아주 중요하다. 근데... 모든 곳에서 고려할 필요는 없다.

