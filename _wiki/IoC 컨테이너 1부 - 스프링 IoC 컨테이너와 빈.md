---
layout: wiki
title: IoC 컨테이너 1부 - 스프링 IoC 컨테이너와 빈
summary:
date: 2022-11-08 00:00:00 +0900
updated: 2022-11-08 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [IoC 컨테이너 1부 - 스프링 IoC 컨테이너와 빈](https://www.inflearn.com/course/spring-framework_core/unit/15506)

## 3. IoC 컨테이너 1부: 스프링 IoC 컨테이너와 빈

### Inversion of Control: 의존 관계 주입(Dependency Injection)이라고도 하며, 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라, 주입 받아 사용하는 방법을 말함.

### 스프링 IoC 컨테이너

- 의존성 주입을 개발자가 직접 할 게 아니라, 편하게 쓰기 위해서 컨테이너라는 공간에 객체를 등록해놓고 편하게 꺼내 쓸 수 있도록 함 (스프링 프레임워크에서 고안됨)
- BeanFactory : 컨테이너를 구현한 인터페이스
- 애플리케이션 컴포넌트의 중앙 저장소.
- 빈 설정 소스로부터 빈 정의를 읽어들이고, 빈을 구성하고 제공한다.

### 빈(Bean)

- 스프링 IoC 컨테이너가 관리 하는 객체.
- 장점
  - 의존성 관리
  - 싱글톤으로 빈을 관리하고 꺼내쓸 수 있게 되어 리소스 절약
    - 스코프
      - 싱글톤: 하나
      - 프로포토타입: 매번 다른 객체
  - 라이프사이클 인터페이스 관리가 유용
    - 예 : @PostConstruct
      - 어떤 빈이 만들어진 후에 추가적인 작업을 할 수 있음

### ApplicationContext

- BeanFactory를 확장하여, 더 다양한 기능을 가진 인터페이스
  - MessageSource : 메시지 소스 처리 기능 (i18n)
  - ResourceLoader : 리소스 로딩 기능
  - 이벤트 발행 기능
