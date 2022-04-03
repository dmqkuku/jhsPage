---
title: "2023-03-31 Java Reflection and OOP"
categories: []
tags: [archieve, Java, Reflection, OOP]
excerpt: "Doesn't Java Reflection break OOP(Data Encapsulation)"
classess: wide
slug: "Reflection1"
published: false
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
<div class="notice" markdown="1">
Class 클래스는 사실 generic class이다.
</div>
<div class="notice" markdown="1">
historical한 이유로. getName 메서드는 array 타입에 대해서 이상해 보이는 이름을 반환한다.
```java
Double[].class.getName(); // "[Ljava.lang.Double;"
```
</div>

VM은 각 타입에 대해서 유니크한 Class 객체를 관리해준다. 즉 == 연산자를 이용해 Class 객체를 비교할 수 있다.
```java
    if(e.getClass() == Employee.class) {

    }
```
instanceof가 있는데 굳이? 라고 생각할 수 있는데. instanceof는 subclass로 비교해도 통과시키는 반면.
이 test는 subclass가 들어올 경우 fail 한다.
```java
    class Father {

    }
    class Son extends Father{

    }
    .
    .
    .
    public static void Main(String args[]){
        Father father = new Father();
        Son son = new Son();
        boolean isInstance = son instanceof Father; // true

        boolean isSameClass = son.getClass() == Father.class; //exception 
        //java: incomparable types: java.lang.Class<capture#1 of ? extends com.Son> and java.lang.Class<com.Father>
    }
```

getConstructor 메서드를 이용해 constructor를 가져오고. newInstance를 호출하여 인스턴스를 찍어낼 수 있다.
단 클래스가 argument가 없는 생성자가 존재하지 않으면, exception을 던진다.
> 따라서 JavaBean을 리플렉션을 이용해 생성해서 주입시키는 경우. 해당 JavaBean에는 default 생성자가 (argument가 없는 생성자) 존재하여야 한다.
```java
    var className = "java.util.Random";

    Class cls = Class.forName(className);
    Object objc = cls.getConstructor().newInstance();
```




<div class="notice" markdown="1">
**Exception에 대해서 알아보자**

**Unchecked Exception / Checked Exception**
1. Checked Exception 의 경우. 컴파일러가 경고해 주는 exception이다.
2. 그러나, NullPointer/ Bounds Error 과 같은 경우는 UncheckedException. 즉 컴파일 시점에 아는 것이 불가능한 exception이다.
</div>
<hr/>
<hr/>

Java Reflection 이 OOP의 원칙 중 하나인 캡슐화(Encapsulation)을 위배한다고 여긴 부분은 다음과 같다.
```java
class Capsule {
    private SecretInfo secretInfo;
}
```
를 꺼내서 읽는 것에 그치면 모르겠지만. 

```java


```


