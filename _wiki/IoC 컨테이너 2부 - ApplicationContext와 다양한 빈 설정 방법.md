---
layout: wiki
title: IoC 컨테이너 2부 - ApplicationContext와 다양한 빈 설정 방법
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

- [IoC 컨테이너 2부 - ApplicationContext와 다양한 빈 설정 방법](https://www.inflearn.com/course/spring-framework_core/unit/15507)

![image](https://user-images.githubusercontent.com/114462413/200602023-a3089966-45ff-4782-af28-cd721c41144b.png)

## 스프링부트의 편리함

- web 선택 -> 관련 라이브러리들 의존성 관리

![image](https://user-images.githubusercontent.com/114462413/200599327-4b147cf7-b993-4ed3-b460-219c3196cca2.png)

![image](https://user-images.githubusercontent.com/114462413/200600250-5de7a19a-09bf-49df-b8af-9ba1e494173d.png)

## IoC 컨테이너 2부: ApplicationContext와 다양한 빈 설정 방법

### 스프링 IoC 컨테이너의 역할

- 빈 인스턴스 생성
- 의존 관계 설정
- 빈 제공

### AppcliationContext 생성

- 설정파일을 xml로 설정할 때 : ClassPathXmlApplicationContext

```java
ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
```

- 설정파일을 java파일로 설정할 때 : AnnotationConfigApplicationContext

  - 주입방식 1 : 함수호출

  ![image](https://user-images.githubusercontent.com/114462413/200603128-15f36e5f-24ec-4332-8093-52000b4afec3.png) -

  - 주입방식 2 : 파라미터로 받아서 넘겨주기
    ![image](https://user-images.githubusercontent.com/114462413/200603182-0f732dc5-e517-4973-b998-208885123052.png) -
  - 주입방식 3 : 의존성 주입을 configuraion에서 직접하지 않고, 서비스 로직에서 꺼내와서 주입

![image](https://user-images.githubusercontent.com/114462413/200604995-a59ac4a2-e296-4d07-b330-09595aca73d1.png)

![image](https://user-images.githubusercontent.com/114462413/200605100-8f9cb44f-1e2e-49fc-b007-c2cd6b3774ee.png)

- 어노테이션 방식으로 설정을 읽어들일 때 : @ComponentScan
  - @ComponentScan 사용
  - basePackage는 문자열로 입력해야 하므로 type safety 이슈가 있을 수 있음
  - basePackageClass : type safety함
  - 해당 클래스가 있는 위치에서부터 @Component 어노테이션이 붙은 클래스들을 모두 빈으로 등록해라.
  - @Service, @Repository 등은 @Component를 포함하고 있음

![image](https://user-images.githubusercontent.com/114462413/200608336-13c988c3-5652-4a63-b631-a00a70e6a718.png)

![image](https://user-images.githubusercontent.com/114462413/200607443-78f95440-db88-4328-b2bb-73d6caea9d5e.png)

- (스프링부트, 앞선 내용들은 Spring에 해당하는 내용들) @SpringBootApplication 사용
  - ApplicationContext를 자동으로 생성
  - @ComponentScan이 포함됨
  - @ComponentScan도 할 필요가 없어짐

![image](https://user-images.githubusercontent.com/114462413/200608932-527e8996-ebae-4af7-9d4d-92fb255d3236.png)

### 빈 설정

- 빈 명세서
- 빈에 대한 정의를 담고 있다.
  - 이름
  - 클래스
  - 스코프
  - 생성자 아규먼트 (constructor)
  - 프로퍼트 (setter)
  - ..

### 컴포넌트 스캔

- 설정 방법
  - XML 설정에서는 context:component-scan
  - 자바 설정에서 @ComponentScan
- 특정 패키지 이하의 모든 클래스 중에 @Component 애노테이션을 사용한 클래스를 빈으로 자동으로 등록 해 줌.
