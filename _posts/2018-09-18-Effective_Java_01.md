---
title: Effective Java 01
categories:
 - Java
tags: Java EffectiveJava
---

## 1. 생성자 대신 정적 팩터리 메소드를 사용할 수 없는지 생각해 보라

summary
```
#정적 팩토리 메소드

정적 팩토리 메소드와 public 생성자는 용도가 다르며, 상황에 따라 어떤 것이 효과적인지 고려한 후 사용해야 한다.

장점
1. 정적 팩토리 메소드에는 이름(name)이 있다.
2. 생성자와는 달리 호출할 때마다 새로운 객체를 생성할 필요는 없다.
3. 생성자와는 달리 반환값 자료형의 하위 자료형 객체를 반환할 수 있다.
4. 형인자 자료형(parameterized type) 객체를 만들 때 편리하다.  

단점
1. public, protected 로 선언된 생성자가 없으므로 하위 클래스를 만들 수 없다.
2. 정적 팩토리 메소드가 다른 정적 메소드와 확연히 구분되지 않는다.
```



#### 모르는 것
 - 클래스의 시그니처 (signature) 란 ?
 - 객체를 Cache 하여 재사용하는 방법 ?
 - 경량 (Flyweight) 패턴이란 ?
 - 개체 통제 클래스 (instance-controlled class)
 - == 연산과 equals 연산의 동작
 - 인터페이스 기반 프레임워크 (interface-based framework)
 - 객체 생성 불가능 (noninstantiable) 클래스
 - 계승 (inheritance) 기법과 구성 (composition) 기법


#### 기억할 것
 - 팩토리 메소드를 생성자 대신 사용하여, public 이 아닌 클래스의 객체를 반환하는 API를 만들 수 있다. (인터페이스를 자료형으로 이용. 이 때 인터페이스는 정적 메소드를 가질 수 없으므로, 인터페이스이름s 라는 이름을 가진 객체생성불가능 클래스를 만들어 정적 팩토리 메소드를 둔다.)  

 - Java 의 컬렉션 프레임워크에는 32개의 컬렉션 인터페이스 구현체가 들어있다. (32개의 public 클래스가 아니다.) -> 정적 팩토리 메소드를 사용했다.  

 - Java 의 EnumSet 에는 public 으로 선언된 생성자가 없으며, 정적 팩토리 메소드 뿐이다. 이 메소드들은 enum 상수 개수에 따라 두 구현체중 하나를 객체로 반환한다. (상수 64개 이하 : RegularEnumSet / 상수 64개 초과 : JumboEnumSet)
  
 - 정적 팩토리 메소드를 이용한 API의 사용자는 반환된 객체가 인터페이스를 정확히 따른다는 사실을 알고 있으므로, 세부적인 구현에 대해서는 생각할 필요가 없다. 또한 내부적으로 구현체를 안전하게 변경할 수 있다. -> 바람직

 - 서비스 제공자 프레임워크(Service Provider Framework / ex.JDBC) 의 근간을 이루는 것이 바로 유연한 정적 팩토리 메소드이다.  

 - 정적 팩토리 메소드를 사용하면 컴파일러가 자료형을 유추 (type inference) 하도록 할 수 있다.
  -> java7 부터 생성자를 호출할 때도 자료형 유추를 사용할 수 있게 되었다.(diamond operator)


>  **서비스 제공자 프레임워크**  

컴포넌트 | 설명 | 예시 (JDBC)
---|---|---
서비스 인터페이스 | 서비스 제공자가 구현 | Connection
제공자 등록 API | 현체를 시스템에 등록하여 클라이언트가 쓸 수 있도록 함 | DriverManager.registerDriver
서비스 접근 API | 클라이언트에게 실제 서비스 구현체를 제공  | DriverManager.getConnection
(서비스 제공자 인터페이스) | 옵션. 서비스 제공자가 구현. 서비스 구현체의 객체를 생성하기 위한 것 | Driver 



 #### 참고

 ```java
//Collections.java

// noninstantiable class - constructor is private

public class Collections {
    // Suppresses default constructor, ensuring non-instantiability.
    private Collections() {
    }

```


```java
// EnumSet.java

/**
    * Creates an empty enum set with the specified element type.
    *
    * @param <E> The class of the elements in the set
    * @param elementType the class object of the element type for this enum
    *     set
    * @return An empty enum set of the specified type.
    * @throws NullPointerException if <tt>elementType</tt> is null
    */
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
    Enum<?>[] universe = getUniverse(elementType);
    if (universe == null)
        throw new ClassCastException(elementType + " not an enum");

    if (universe.length <= 64)
        return new RegularEnumSet<>(elementType, universe);
    else
        return new JumboEnumSet<>(elementType, universe);
}

```