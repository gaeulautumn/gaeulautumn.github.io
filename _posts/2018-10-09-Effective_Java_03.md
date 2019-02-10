---
title: Effective Java 03
categories:
 - Java
tags: Java EffectiveJava
---

## 3. private 생성자나 enum 자료형은 싱글턴 패턴을 따르도록 설계하라

summary
```
#싱글턴 패턴
- 싱글턴은 객체를 하나만 만들 수 있는 클래스다.

단점
- 테스트가 어려울 수 있다.

```



#### 기억할 것

- 싱글턴 구현 방법

1. 생성자는 private 으로 선언하고, 싱글턴 객체는 static 멤버를 통해 이용. (JDK 1.5 이전)
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis(){...}

    public void leaveTheBuilding(){...}
}
```
이 경우, reflection 기능을 통해 private 생성자를 호출할 수도 있으므로, 방어하기 위해서는 생성자를 고쳐야 한다. (ex.두번째 객체를 생성하라는 요청을 받으면 예외처리)


2. public으로 선언된 정적 팩터리 메서드를 이용. (JDK 1.5 이전)
```java
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis(){...}
    public static Elvis getInstance() {return INSTANCE;}

    public void leaveTheBuilding(){...}
}
```

위 방법들로 구현한 싱글턴 클래스를 Serializable 클래스로 만들려면, (Deserialize 될 때마다 새로운 객체가 생기지 않게 하려면) Elvis 클래스에 readResolve 메소드를 추가해야 한다.
```java
private Object readResolve() {
    // 동일한 Elvis 객체가 반환되게 하고, 가짜 Elvis 객체는 GC가 처리하도록 함
    return INSTANCE;
}
```


3. 원소가 하나뿐인 Enum 자료형 정의 (JDK 1.5 이후)

```java
public enum Elvis {
    INSTANCE;

    publlic void leaveTheBuilding(){...}
}
```

기능적으로는 public 필드를 사용하는 구현법과 동일하나, 좀 더 간결하고, 직렬화가 자동으로 처리되며, 리플렉션을 통한 공격에도 안전하다.
**원소가 하나뿐인 enum 자료형이야 말로 싱글턴을 구현하는 가장 좋은 방법이다.**