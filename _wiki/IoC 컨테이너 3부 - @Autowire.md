---
layout: wiki
title: IoC 컨테이너 3부 - @Autowire
summary:
date: 2022-11-10 00:00:00 +0900
updated: 2022-11-10 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [IoC 컨테이너 3부 - @Autowire](https://www.inflearn.com/course/spring-framework_core/unit/15508)

## IoC 컨테이너 3부: @Autowire

- 필요한 의존 객체의 “타입"에 해당하는 빈을 찾아 주입한다.

### @Autowired

- required: 기본값은 true (따라서 못 찾으면 애플리케이션 구동 실패)

### 사용할 수 있는 위치

- 생성자 (스프링 4.3 부터는 생략 가능)
- 세터
- 필드

### 결론

- 같은 타입의 여러 클래스의 빈을 등록하는 경우, 주입하고자 하는 클래스에 @Primary를 사용하는 것을 추천한다.

### 경우의 수

- 해당 타입의 빈이 없는 경우
  - 애플리케이션 구동 실패
  - ![image](https://user-images.githubusercontent.com/114462413/200871630-f208b09e-e422-4821-a793-6fd4712a01d1.png)
  - setter 주입을 사용하는 경우에도 @Autowired를 사용하면 해당 빈을 컨테이너에서 찾기를 시도하고, 없으면 애플리케이션 구동이 실패한다 (required = True가 기본값이므로)
  - 따라서 이럴땐 required = false로 설정하면, bookRepository 인스턴스를 생성해서 setter로 주입하는 방식으로 애플리케이션을 구동할 수 있다.
  - ![image](https://user-images.githubusercontent.com/114462413/200873585-b187ed99-9a5f-45c1-914f-959a0d72c429.png)
  - 필드 주입의 경우에도 마찬가지로 required=false로 설정하므로써 BookRepository가 빈으로 등록되어 있지 않더라도 주입해서 애플리케이션을 구동할 수 있다.
  - ![image](https://user-images.githubusercontent.com/114462413/200874564-95259cfa-df8b-470e-bb24-129aaf592ec5.png)
- 해당 타입의 빈이 한 개인 경우

  - ![image](https://user-images.githubusercontent.com/114462413/200871964-cd460afe-aa25-4cf7-b87d-67a67a738c26.png)
  - ![image](https://user-images.githubusercontent.com/114462413/200872136-7b3e2fed-6327-4512-adb1-94804e5feb06.png)

- 해당 타입의 빈이 여러 개인 경우
  - 빈 이름으로 시도,
    - 같은 이름의 빈 찾으면 해당 빈 사용 : 필드변수명을 빈으로 생성되는 클래스 이름(카멜케이스)와 동일하게 주면, 해당 빈을 찾음
      ![image](https://user-images.githubusercontent.com/114462413/200880415-11328926-e22c-475d-a78b-b29473ccfcab.png)
    - 같은 이름 못 찾으면 실패
      - 다음 목차

### 같은 타입의 빈이 여러개 일 때 (필드변수명과 같은 유일한 빈을 찾지 못한 경우)

- 케이스 : BookRepository를 인터페이스로, 그 구현체가 2개가 있으며 빈으로 등록되어 있을 때, 서비스에서 BookRepository를 Autowire해서 주입받고자 한다면, 둘 중 어떤 것을 골라야 할 지 모르기 때문에 에러가 발생

![image](https://user-images.githubusercontent.com/114462413/200875502-15f01e17-dc0a-4d7f-9997-8be7a3acaa06.png)

![image](https://user-images.githubusercontent.com/114462413/200875590-c0d6285f-f6ea-433d-974f-306933cf348b.png)

![image](https://user-images.githubusercontent.com/114462413/200875640-8caadce7-4ccc-48b8-8f56-9a473f8109ea.png)

![image](https://user-images.githubusercontent.com/114462413/200875675-b82b468f-6ded-4590-9ca4-d55a88190d51.png)

- 스프링에서는 3가지 해결책을 제시한다.
  ![image](https://user-images.githubusercontent.com/114462413/200876662-12cdcb75-d936-4a1a-9f0b-8b3fd84e23ad.png)

  1. @Primary

  - 이 어노테이션이 붙은 빈을 가져온다
  - 스프링 러너 구현해서 아래와 같이 테스트해볼 수 있다.
    ![image](https://user-images.githubusercontent.com/114462413/200877752-640bbf13-fa7e-48ef-9ab7-7d5458d29437.png)
    ![image](https://user-images.githubusercontent.com/114462413/200877193-eb9d7b31-e06d-4a0b-ad0e-7079be972b32.png)
    ![image](https://user-images.githubusercontent.com/114462413/200878023-67f158d6-1bf1-4d4c-9fb5-286c9428d09b.png)

  2. 해당 타입의 빈 모두 주입 받기
     ![image](https://user-images.githubusercontent.com/114462413/200879678-d1f7c1a8-d582-4a38-891b-1bc3e6d91cc3.png)

  3. @Qualifier (빈 이름으로 주입)

  - @Primary와는 다르게, 주입을 하는, @Autowired가 사용되는 부분에서 어떤 것을 사용할 지 명시하는 방식
  - 빈은 클래스명을 카멜케이스로 해서 자동생성되는데, 이 클래스명을 명시해줘서 사용함
  - 문자열 입력으로 type safety하지 않아서, @Primary가 주로 사용됨
    ![image](https://user-images.githubusercontent.com/114462413/200878795-37973b0b-da56-46db-819c-d041c148420d.png)

### 동작 원리

- 첫시간에 잠깐 언급했던 빈 라이프사이클 기억하세요?
- BeanPostProcessor

  - 새로 만든 빈 인스턴스를 수정할 수 있는 라이프 사이클 인터페이스
  - @Primary, @Autowired 등 라이프 사이클 로직을, 다른 빈들에게 적용하는 역할

- AutowiredAnnotationBeanPostProcessor extends BeanPostProcessor
  - 스프링이 제공하는 @Autowired와 @Value 애노테이션 그리고 JSR-330의 @Inject 애노테이션을 지원하는 애노테이션 처리기.
  - ApplicationContext (빈 컨테이너)에 자동으로 빈으로 등록되어 있음
