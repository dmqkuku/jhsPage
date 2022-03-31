---
title: "2023-03-31 Java Reflection and OOP"
categories: []
tags: [archieve, Java, Reflection, OOP]
excerpt: "Doesn't Java Reflection break OOP(Data Encapsulation)"
classess: wide
---

참고 : <a href="/jhsPage/diary/2022/03/30/">2022-03-30 개발 일기 Day3</a>

<hr/>
<hr/>

**What is Reflection?**
참고 : https://www.amazon.com/Core-Java-I-Fundamentals-11th-Horstmann/dp/0135166306

Reflection library가 존재하기에, ORM, IOC가 가능하다.


클래스의 capability(능력 == property, method, constructor...)를 분석할 수 있는 프로그래을 reflective한 프로그램이라고 부른다.


Java 프로그램이 동작하는 동안, Java Runtime은 항상 Runtime Type identification을 모든 객체에 유지한다.
이 정보는 각 객체가 어떤 클래스에 속해 있는지 분간하기 위해 사용되어. VM으로 하여금 올바른 메소드를 호출할 수 있도록 한다.


Class 타입이 곧 이 정보이며. Object에 대해 getClass를 호출함으로서 이 타입의 인스턴스를 얻을 수 있다.
```java
    Employee e;
    Class cls = e.getClass();
```
이 때 얻어지는 class 객체에는 해당 인스턴스에 대한 정보가 들어있다.


<div class="notice" markdown="1">
**tip**
프로그램 시작시, Main 메서드를 포함한 클래스가 로드될 때, 필요한 모든 클래스가 로드된다.각각의 클래스에 필요한 클래스도 따라 로드된다.


아주 커다란 어플리케이션에서 이 과정은 아주 오래 걸릴 수 있는데. 만약 main 메서드를 포함한 클래스가 명시적으로 다른 클래스를 refer to 하지 않을 경우. Class.forName으로 다른 클래스 로딩을 강제할 수 있다.
</div>
Class 클래스는 사실 generic class이다.

<hr/>
<hr/>

Java Reflection 이 OOP의 원칙 중 하나인 캡슐화(Encapsulation)을 위배한다고 여긴 부분은 다음과 같다.
```java
class Capsule {
    private SecretInfo secretInfo;
}
```
를 꺼내서 읽는 것에 그치면 모르겠지만. 


