---
title: "2023-04-03 Is Exception Bad"
categories: []
tags: [archieve, Java, Exception]
excerpt: "Is Exception Bad"
classess: wide
slug: "Exception2"
published: false
---

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

