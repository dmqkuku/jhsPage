---
title: "2023-03-31 Java Reflection and OOP"
categories: []
tags: [archieve, Java, Reflection, OOP]
excerpt: "Doesn't Java Reflection break OOP(Data Encapsulation)"
classess: wide
slug: "Reflection1"
published: true
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


참고 : <a href="/jhsPage/archieve/2022/03/31/exception1">2022-03-31 Java Exception</a>
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
Capsule capsule = new Capsule();
Capsule capsule2 = new Capsule();
//모든 Capsule클래스의 secretInfo 필드가 접근 가능하도록 변경됨.
capsule.getClass().getDeclaredFields("secretInfo").setAccesible(true); 
capsule.secretInfo = ...;
capsule2.secretInto = ...;
```
그러면~ 스프링 내부에 private field를 요렇게 접근해서 바꿀 수 있을까?

> 가능하다고 생각한다. 실험은 스프링 프레임워크 내부에 테스트 해볼 만한 private 필드를 찾으면 수행하도록 하겠다.


Core Java Volumn 1에 의하면. 
setAccesible은 accessiblity를 바꿀 수 없을 때 exception을 던진다. accesbility 변경은 java module system이나 security manager에 의해 막힐 수 있다.

Java9부터 단계적으로 JavaAPI의 모듈화가 이루어졌다. Java11에서 module화된 JDK 라이브러리 APi에 직접 접근 할 경우. Error!

워낙에 Java에서 reflection을 자주 사용하므로. Java9와 Java10에서 module안의 nonpublic 요소의 접근성을 변경하면. warning만 내뱉는다.


## Java Reflection. Encapsulation. Java 9~

Java8.이후로 진행된 중대한 차이점.
In Java 8 and before, you can call public methods on any public class you like, both directly and reflectively.
After the Java module system arrived, these calls became subject to additional restrictions.
참고 : <a href="https://blogs.oracle.com/javamagazine/post/a-peek-into-java-17-continuing-the-drive-to-encapsulate-the-java-runtime-internals"> 오라클 articles. A peek into Java 17</a>

Java8에서 다음 코드를 컴파일 할 경우. warning!
```java
import sun.net.URLCanonicalizer;

public class MyURLHandler extends URLCanonicalizer{
    public boolean isSimple(String url){
        return isSimpleHostName(url);
    }
}
```
```
src/ch02/MyURLHandler.java:3: warning: URLCanonicalizer is internal proprietary API and may be removed in a future release
import sun.net.URLCanonicalizer;
               ^
 src/ch02/MyURLHandler.java:5: warning: URLCanonicalizer is internal proprietary API and may be removed in a future release
 public class MyURLHandler extends URLCanonicalizer {
```
만약 Java 11에서 컴파일 한다면?
```
src/ch02/MyURLHandler.java:3: error: package sun.net is not visible
import sun.net.URLCanonicalizer;
          ^
  (package sun.net is declared in module java.base, which does not export it to the unnamed module)
src/ch02/MyURLHandler.java:8: error: cannot find symbol
        return isSimpleHostName(url);
               ^
  symbol:   method isSimpleHostName(String)
  location: class MyURLHandler
```
Not Warning! Error!

Java 9와 그 이상을 Modular JDKs라고 부름

modular JDKs에서는 exported packages만 접근 가능하다.

더이상! public 멤버가 모든 코드에서 접근 가능하지 않다.

JDK 패키지에서, java.혹은 javax.로 시작하는 JDKs 패키지만 public API고 나머지는 internal only이다.

여기서 Reflection의 setAccesible은 진짜 커다란 구멍이 된다.

이에 대처하기 위해 modular JDKs에서는 module-info.class에 reflective policy를 정의할 수 있다.

1. 모든 module은 기본적으로 exported인 패키지만 접근 가능하다. 이는 reflection도 포함이다.
2. 만약 만약 reflection을 통해 exported가 아닌 패키지에도 접근 가능해야 한다고 생각하는 모듈의 경우. module-info.class에 opens를 명시하여 접근할 수 있도록 열어놓을 수 있다.
3. 만약 특정 packages에만 열려있고 싶으면, opens ... to ... 로 지정할 수 있다.


정리하자면

java8 : 직접 접근과 reflective 접근 모두 가능 

java11 : 모듈로 직접 접근 거부 가능. reflective로 접근 모두 가능.

java16 : 모듈로 직접 접근 거부 가능. reflective로 접근 기본적으로 거부. cli에서 예전 동작으로 구동시키면 가능

java17 : 둘 다 불가능.