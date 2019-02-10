---
title: Effective Java 04
categories:
 - Java
tags: Java EffectiveJava
---

## 4. 객체 생성을 막을 때는 private 생성자를 사용하라

summary
```
#private 생성자
- 객체를 만들 필요가 없는 클래스(ex. java.lang.Math, java.util.Collections 와 같은 유틸클래스) 는 생성자를 private 으로 만든다.
```



#### 기억할 것

- 객체를 만들 수 없도록 하려고 클래스를 abstract 로 선언해도 소용 없다. 하위 클래스를 정의하는 순간 객체 생성이 가능해진다.
- 생성자를 private 으로 하면 하위클래스를 만드는 것도 불가능하다.
