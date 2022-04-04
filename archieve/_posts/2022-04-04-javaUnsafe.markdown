---
title: "2023-04-04 Java Unsafe"
categories: []
tags: [archieve, Java, Unsafe]
excerpt: "Unsafe"
classess: wide
slug: "unsafe1"
published: true
---

참고 : <a href="https://blogs.oracle.com/javamagazine/post/the-unsafe-class-unsafe-at-any-speed">The Unsafe Class: Unsafe at Any Speed</a>

참고 : <a href="http://mishadoff.com/blog/java-magic-part-4-sun-dot-misc-dot-unsafe/">Java Magic. Part 4: sun.misc.Unsafe</a>

가아끔 우리는 프로그래밍하면서 rule를 부서야 할 때가 존재한다.
Java에서 해당 방법은 reflection, class loading(including associated bytecode transformation), Unsafe. 세가지 이다.

이 세가지 중 Unsafe는 다른 방법으로는 불가능하고, 플랫폼의 룰을 깰 수 있는 기능을 제공하므로, 가장 위험하다.


1. directly access CPU and other hardware features
2. create an object but not run its constructor
3. create a truly anonymous class without the ususal verifiaction
4. manually manage off-heap memory

이 클래스는 sun.misc 하위에 위치해 있으며, java8 까지는 접근이 가능하고, java9 이후 버전에서는 module화 되어 접근을 제한하고 있다.


사실 많은 java 프레임워크(spring도?)가 일부 혹은 많은 기능을 unssafe에서 가져오고 있다.
다음 코드
```java
public final class SynchronizedCounter implements Counter {
    private int i = 0;

    @Override
    public synchronized int increment(){
        return i = i + 1;
    }
    @Override
    public synchronized int get(){
        return i;
    }
}
```
를 "Compare and Swap" == CAS를 이용한 코드로 바꾸어 보자(해당 기능은 Java의 메모리 모델의 일부가 아니고, 모던 CPU의 기능이다.)

```java
public final class AtomicCounter implements Counter{
    private static final Unsafe unsafe;
    private static final long valueOffset;

    //volatile thread unsafe한 컴파일러 최적화를 수행하지 않는다.
    private volatile int value = 0;
    static {
        try{
            Field f = Unsafe.class.getDeclaredField("theUnsafe");
            f.setAccesible(true);
            unsafe = (Unsafe)f.get(null);
            valueOffset = unsafe.objectFieldOffset(AtomicCounter.class.getDeclaredField("value"));
        }catch(Exception ex){
            throw new Error(ex);
        }
    }
    @Override
    public int increment(){
        return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    }
    @Override
    public int get(){
        return value;
    }
}
```


<div class="notice" markdown="1">
**What is CAS? - Compare and Swap**


요약 :  기존값 Old에 대해 새 값 New와 값의 주소 p가 주어질 경우. *p와 Old가 같은 경우 New를 Old와 swap한다.


```python
//의사코드
function cas(p : pointer to int, old : int, new : int) is
    if *p != old
        return false;

    *p <- new;

    return true;
```


기본적으로 concurrent algorithms를 구현할 때. check-then-act의 형태가 된다. 단! check와 act가 모두 동일한 atomic 맥락에서 실행되어야 한다.


```java
class Singleton {
    private static INSTANCE = null;
    public Singleton getInstance(){
        if(INSTANCE == null){
            Synchronized(Singleton.class){
                if(INSTANCE == null){
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
//엄밀히 말해서. 이건 double check긴 함.
```

concurrent Algorithms의 편린을 이해하기 위해. Atomic이 무었인지 알아보자.


Atomic : 해당 코드 블록을 실행하는 쓰레드는 다른 어떠한 쓰레드의 방해없이 해당 코드 블록 실행을 마칠 수 있다.
어떠한 다른 쓰레드도 해당 atomic 블록을 동시에 실행할 수 없다.


Java 에서는 보통 이를 synchronized 키워드로 구현한다. 


그러나! 특정 thread가 A코드 블록을 run하는 동안 다른 thread를 블록하는 것은 비용이 든다!



1. 그 시간 동안 기다려야 하므로, 시간의 낭비.
2. 해당 코드A를 다른 thread가 실행을 종료한 후 즉시 다른 thread가 실행되지 못한다. 그 사이 시간의 낭비.


2번은 eager waiting일 경우 최소화 되지만, eager는 그 자체로 계속 thread가 진입 시도를 해야 하므로. 역시 자원이 낭비된다.


보통 modern CPU는 atomic CAS 연산을 내장하고 있다. 전체 CPU 코어에 걸쳐서 단 하나의 쓰레드가 해당 코드를 실행하도록 지원한다.!!!


Java Concurrency에 대해서는 다음에 더 다루도록 하겠다.


참고 : <a href="https://jenkov.com/tutorials/java-concurrency/compare-and-swap.html">Compare and Swap</a>

참고 : <a href="https://en.wikipedia.org/wiki/Compare-and-swap#:~:text=In%20computer%20science%2C%20compare%2Dand,to%20a%20new%20given%20value.">Compare-and-swap</a>
</div>