---
layout: wiki
title: Inversion_of_Control
summary:
date: 2022-10-31 00:00:00 +0900
updated: 2022-10-31 00:00:00 +0900
tag: Spring
toc: true
public: true
parent: [[WhiteShip]]
latex: true
---

## 참고문헌

- [IoC 소개](https://www.inflearn.com/course/spring/unit/15538)
- 백기선님 강의자료를 배꼈습니다. 좋은 강의 감사합니다.

## Inversion of Control : 제어가 바뀌었다?

### "내가 쓸 놈은 내가 만들어 쓸께" : 일반적인 의존성에 대한 제어권

```java
class OwnerController {
    private OwnerRepository repository = new OwnerRepository();
}
```

### “내가 쓸 놈은 이 놈인데... 누군가 알아서 주겠지” : IoC

- 내가 쓸 놈의 타입만 맞으면 어떤거든 상관없지 뭐.. .
- 그래야 내 코드 테스트 하기도 편하지.
- 일단 필요한 건 누가 넣어줄 거라 생각하고, 있다고 가정하고 쓴다.

```java
class OwnerController {
    private OwnerRepository repo;

    public OwnerController(OwnerRepository repo) {
        this.repo = repo;
    }
// repo를 사용합니다.
}

class OwnerControllerTest {
    @Test
    public void create() {
        OwnerRepository repo = new OwnerRepository();
        OwnerController controller = new OwnerController(repo);     //필요한 걸 넣어줌
    }
}
```
