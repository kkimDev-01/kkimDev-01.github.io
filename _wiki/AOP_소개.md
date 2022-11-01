---
layout: wiki
title: AOP_소개
summary:
date: 2022-11-02 00:00:00 +0900
updated: 2022-11-02 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [AOP 소개]](https://www.inflearn.com/course/spring/unit/15543)

## AOP 소개

### 흩어진 코드를 한 곳으로 모아

- 흩어진 코드를 한 곳으로 모으고
- 다른 클래스들은 자신이 해야할 일만 하도록 (Single_Responsibility_Principle, 단일책임 원칙)

```java
//흩어진 AAAA 와 BBBB

class A {
    method a () {
        AAAA
        오늘은 7월 4일 미국 독립 기념일이래요.
        BBBB
    }
    method b () {
        AAAA
        저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
        BBBB
    }
}

class B {
    method c() {
        AAAA
        점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
        BBBB
    }
}

//모아 놓은 AAAA 와 BBBB

class A {
    method a () {
        오늘은 7월 4일 미국 독립 기념일이래요.
    }
    method b () {
        저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
    }
}
class B {
    method c() {
        점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
    }
}
class AAAABBBB {
    method aaaabbb(JoinPoint point) {
        AAAA
        point.execute()
        BBBB

    }
}
```

## AOP 적용 예제

### @LogExecutionTime 으로 메소드 처리 시간 로깅하기

- https://github.com/kkimDev-01/spring-petclinic/commit/3fab961c1a544fc79485309d0d98770b77887933
