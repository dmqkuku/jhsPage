---
title: "2023-03-31 Java Exception"
categories: []
tags: [archieve, Java, Exception]
excerpt: "Java Exception"
classess: wide
slug: "Exception1"
published: false
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

