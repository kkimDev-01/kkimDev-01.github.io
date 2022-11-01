---
layout: wiki
title: 의존성_주입(Dependency_Injection)
summary:
date: 2022-11-01 00:00:00 +0900
updated: 2022-11-01 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [의존성 주입](https://www.inflearn.com/course/spring/unit/15541)

## 의존성 주입 (Dependency Injection)

### 필요한 의존성을 어떻게 받아올 것인가

### @Autowired / @Inject를 어디에 붙일까?

### 1. 생성자

- 생성자가 하나이면 @Autowired가 없어도 생략된 것이다.
- 생성자가 두 개 이상이면 생성자에 @Autowired를 붙인다.

```java
    @Controller
    class VisitController {

        private final VisitRepository visits;
        private final PetRepository pets;

        public VisitController(VisitRepository visits, PetRepository pets) {
            this.visits = visits;
            this.pets = pets;
	}

```

```java
    @Controller
    class VisitController {

        private final VisitRepository visits;
        private final PetRepository pets;

        @Autowired
        public VisitController(VisitRepository visits, PetRepository pets) {
            this.visits = visits;
            this.pets = pets;
	    }
    }
```

### 2. 필드

```java
    @Controller
    class VisitController {

        @Autowired
        private VisitRepository visits;

        @Autowired
        private  PetRepository pets;

	}
```

### 3. setter

```java
    @Controller
    class VisitController {

        private VisitRepository visits;

        private PetRepository pets;

        @Autowired
        public void setVisitRepository(VisitRepository visits) {
            this.visits = visits;
        }

        @Autowired
        public void setPetRepository(PetRepository pets) {
            this.pets = pets;
        }

	}
```

## 권장

- 이 클래스에서 반드시 필요한 객체들인 경우 : 생성자 주입
- setter 없으면, 필드 주입
  - setter가 없는데 굳이 setter를 쓰게 되면, 안바뀌어야 될 애들이 추후에 바뀔 수 있는 문제가 있다.
  - 필드 주입은 한번 @Autowired로 받고 안바뀜
- setter 있으면, 그때는 setter 주입 사용
