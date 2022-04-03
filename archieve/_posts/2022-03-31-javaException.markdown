---
title: "2023-03-31 Java Exception"
categories: []
tags: [archieve, Java, Exception]
excerpt: "Java Exception"
classess: wide
slug: "Exception1"
published: true
---

**주의! Exception을 사용하는 것이 올바르냐는 논쟁이 존재함.**

**해당 논쟁에 대한 개인적인 의견은 다음 포스트에서 밝히겠다.**

**이 Post에서는 일단 Exception을 사용한다고 가정하고 진행.**

새상에는 happy Path가 아닌 경우가 더 많다.
우리는 언제나 유저의 bad input과 buggy code를 신경써야 한다.

이런 상황에서 Exceptional condition(error)를 마주치면, 
1. 사용자에게 error의 발생을 알리고,
2. 그때까지의 모든 work를 저장하고
3. gracefully exit를 시행한다.
<div class="notice" markdown="1">
**What is Gracefully Exit?**
일종의 프로그래밍 idiom(관습적으로 부르는 무언가)으로 프로그램이 심각한 error를 마주친 상황에서, 주어진 절차에 따라 진행하여 프로그램을 종료하는 것을 가르킨다고 한다.


예기 치 못한 오류로 프로그램이 "리소스 정리도 제대로 못하고 뻗는다 VS 정리하고 종료한다."의 차이라고 보면 되겠다.


근데 심각한 오류라면 graecfully Exit이 가능한가???
</div>

## Dealing with Errors

가능한 exception이 날 수 있는 경로 들을 살펴보자
1. 사용자가 개떡같이 입력할 경우
2. 기기가 갑자기 disconnect 되는 경우 등.
3. 디스크가 꽉 차는 경우 등.
4. 코드에서 나는 에러

Exception 대신에 error Code를 리턴하게 할 수도 있지만...

Error Code는 단점이 있다. 
1. input data가 올바른지 아닌지 판별하는 것이 불가능할 수도 있다.?
2. error code가 valid result일 수도 있다.

 
Exception을 사용하면. 즉각 method에서 탈출하게 된다. 즉각 error 정보를 object에 감싸서 던지면서.

<div class="notice" markdown="1">
이게 어떻게 가능한거임? 그니까 코드의 어느 시점이든 throw new Exception() 따위의 코드가 추가되면 바로 나가진다는 이야기인데...


**Exception의 원리와 그 비용에 관하여**

일단 Exception의 비용은 탈출시. Object를 만들기 때문에 발생한다는 주장과(참조 : https://adtmag.com/articles/2000/08/22/effective-exception-handling-in-java.aspx)
그보다는 StackTrace를 추적하고, 기록하는 과정에서 발생한다는 주장이 있음. (참조 : https://www.baeldung.com/java-exceptions-performance#:~:text=In%20Java%2C%20exceptions%20are%20generally,be%20used%20for%20flow%20control.)


참조 : https://stackoverflow.com/questions/299068/what-are-the-effects-of-exceptions-on-performance-in-java 이 글과 같이 원인을 assembly나 bytecode 레벨에서 찾는 경우도 있음.

하나씩 살펴보자.
 </div>

자바 언어에서. 모든 exception 객체는 Throwable 의 자식이다.

Throwable  ->  Error
           ->  Exception  ->  IOException
                          ->  Runtime Exception

RuntimeException은 내가 코드를 잘못짜서 생기는 거고,
If it is a RuntimeException, it was your fault


**Error나 RuntimeException의 하위 클래스들은 전부 UncheckedException이며, 나머지는 전부 CheckedException이다.**

## Declaring Checked Exceptions



자바 메서드는 스스로 handle할 수 없는 상황에 맞닥뜨렷을 때. exception을 던져야 한다.

<div class="notice" markdown="1">
**생각하기**
Exception을 사용하지 마라고 하는 분들은 이경우 어떻게 대처할까?


> error Code return?


> 어거지로라도 돌리기?
</div>


Exception이 발생하는 경우는 크개 두 카테고리로 나뉜다.


1-1. checked Exception을 던지는 method를 사용하는 경우.


1-2. 에러를 내부 구현에서 명시적으로 throw 하는 경우


2-1. 프로그래밍 에러. 0으로 나누기. 배열을 다루다 out of boundary 


2-2. JVM이나 Runtime Library에서...


이 4가지 중에서. "1-"에 해당하는 경우. 다른 사용자를 위해, 이 메서드를 사용하면 Exception이 던져질 것이라고 알려주어야 하고. 이때 사용되는 것이 throws.


결론적으로 모든 메서드는 내부에서 발생하는 Exception을 어떠한 방식으로든 처리하여야 한다.
그리고 이 처리 방법에는 "다시 던지는 방법"과 "내부에서 catch하는 방법" 두가지가 존재한다.


**주의**
메서드를 오버라이드 할 경우. subclass에서는 수퍼 클래스의 메서드가 던지는 checked exception보다 general한 exception을 던질 수 없다.
특히 오버라이드 할 메서드가 exception을 던지지 않을 경우. 그를 오버라이드한 서브 클래스의 메서드는 exception을 던질 수 없다.






<div class="notice" markdown="1">
**C#은 Exception이 한 종류.**
**Exception이 캐치되지 않고, 끝까지 올라올 경우. JVM이 책임지고 프로그램 종료.**
**Java는 임베디드 기기에 탑재되려고 만들어진 언어...**
**C같은 언어에서는 Exception이 존재하지 않는다.**
**웹 서버는 프로그램(서버)가 실행되는 동안. 문제가 발생했을 때. 대처할 사람이 없을 가능성이 크다. 실제로 주말이든 밤이든 이유는 충분. 겪어봄...**
**0으로 나누는 연산 따위를 요즘 시스템에서 수행할 경우. CPU에서 인터럽트/시그널 발생. -> OS에서 캐치하여 프로그램 종료...**
**JVM Exception의 안전성은 OS에서 대신 해주고 있다.**
</div>