---
title: Effective Java 06
categories:
 - Java
tags: Java EffectiveJava
---

## 6. 유효기간이 지난 객체 참조는 폐기하라

summary
```
- 참조되지 않는 객체로 인한 메모리 누수가 발생하지 않도록 주의해야 한다.
```



#### 기억할 것

- GC 에 익숙해져 메모리 관리가 필요하다는 사실을 잊으면 안된다.
- 더이상 참조하지 않는 객체(**만기 참조 obsolete reference**) 는 null 로 만들어 GC가 수집할 수 있게 해준다.
  실수로 객체 참조를 계속 유지할 경우, **의도치 않은 객체 보유 unintentional object retention** 이 발생할 수 있다.
- 하지만 모든 객체를 사용하고 나서 null 로 만들면 코드만 난잡해진다. 예외적인 상황에서만 사용해야 하며, 이 방법을 사용하지 않는 더 좋은 방법은 객체를 선언할 떄 scope 를 최대한 작게 만드는 것이다. 해당 scope 를 벗어난 객체는 자연스럽게 GC 대상이 될 것이다.

메모리 누수가 흔히 발생할 수 있는 곳을 정리하면 다음과 같다.

1. 자체적으로 관리하는 메모리가 있는 클래스
2. 캐시(cache)
 - 객체 참조를 캐시 안에 넣어놓고 잊어버리는 일이 많다.
   WeakHashMap 를 가지고 캐시를 구현하면, 캐시 바깥에서 key 를 참조하고 있을 때만 value 를 보관하므로 key 에 대한 참조가 만기 참조가 되면 key-value 는 자동으로 삭제된다.
   또는 주기적으로 캐시에 저장된 오래된 값들을 지워줘야 한다.
   (Timer, ScheduledThreadPoolExecutor, LinkedHashMap.removeEldestEntry)

3. 리스너 등의 콜백

메모리 누수는 수년간 시스템에 남아있기도 한다. 이는 보통 주의깊게 코드를 검토하다가 발견되거나, 힙 프로파일러 (Heap Profiler) 같은 도구를 통해 발견된다. 
개발할 때는 항상 이런 문제가 발생할 수 있다는 것을 인지하고 대책을 세워야 한다.




