---
layout: wiki
title: IoC 컨테이너 4부 - @Component와 컴포넌트 스캔
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

- [IoC 컨테이너 4부 - @Component와 컴포넌트 스캔](https://www.inflearn.com/course/spring-framework_core/unit/15509)

# 컴포넌트 스캔 주요 기능

- 스캔 위치 설정
  - ![image](https://user-images.githubusercontent.com/114462413/201118569-f1f1db1a-9a59-4113-97c1-f0dad8e4fe9a.png)
  - basePackage 밖의 클래스들은 빈으로 스캔되지 않는다.
- 필터: 어떤 애노테이션을 스캔 할지 또는 하지 않을지

# @Component

- @Repository
- @Service
- @Controller
- @Configuration

# 동작 원리

- @ComponentScan은 스캔할 패키지와 애노테이션에 대한 정보
- 실제 스캐닝은 ConfigurationClassPostProcessor라는 BeanFactoryPostProcessor에 의해 처리 됨.

# function을 사용한 빈 등록

- reflection이나 프록시 기반 라이브러리를 사용하지 않기 때문에, 구동시간을 줄일 수 있다.
- 거의 사용되진 않음 (일일이 등록이 너무 힘들기 때문) : 왜 컴포넌트 스캔 방식이 나왔는지의 이유임. 불편하기 때문

```java
public static void main(String[] args) {
 new SpringApplicationBuilder()
 .sources(Demospring51Application.class)
 .initializers((ApplicationContextInitializer<GenericApplicationContext>)
applicationContext -> {
 applicationContext.registerBean(MyBean.class);
 })
 .run(args);
 }
```

- 위 : 빌더패턴
- 아래 : 일반

```java

@SpringBootApplication
public class DemospringApplication {
   @Autowired
   MyService myService;

   public static void main(String[] args) {
       var app new SpringApplication(DemospringApplication.class);
       app.addInitializers((ApplicationContextInitializer<GenericApplicationContext>) ctx -> {
           if (조건문) {
               ctx.registerBean(Myservice.class);
           }

           //Supplier 등록
           ctx.registerBean(ApplicationRunner.class, () -> args1 -> System.out.println("Funtional Bean Definition!!"))

       });

       app.run(args);
   }//main
}
```
