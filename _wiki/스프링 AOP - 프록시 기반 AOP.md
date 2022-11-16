---
layout: wiki
title: 스프링 AOP - 프록시 기반 AOP
summary:
date: 2022-11-16 00:00:00 +0900
updated: 2022-11-16 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [스프링 AOP - 프록시 기반 AOP](https://www.inflearn.com/course/spring-framework_core/unit/15526)

## 스프링 AOP 특징
-  **프록시 기반의 AOP** 구현체
-  **스프링 빈에만 AOP를 적용**할 수 있다.
- 모든 AOP 기능을 제공하는 것이 목적이 아니라, 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제에 대한 해결책을 제공하는 것이 목적.

## 프록시 패턴
- 왜? (기존 코드 변경 없이) 접근 제어 또는 부가 기능 추가

![image](https://user-images.githubusercontent.com/114462413/201974552-c36928cb-9105-4535-8e22-ade3424ae319.png)

- 기존 코드를 건드리지 않고 성능을 측정해 보자. (프록시 패턴으로)

## 문제점
- 매번 프록시 클래스를 작성해야 하는가?
- 여러 클래스 여러 메소드에 적용하려면?
- 객체들 관계도 복잡하고...

## 그래서 등장한 것이 스프링 AOP
- 스프링 IoC 컨테이너가 제공하는 기반 시설과 Dynamic 프록시를 사용하여 여러 복잡한 문제 해결.
- 동적 프록시: 동적으로 프록시 객체 생성하는 방법
   -  자바가 제공하는 방법은 인터페이스 기반 프록시 생성.
   - CGlib은 클래스 기반 프록시도 지원.
- 스프링 IoC: 기존 빈을 대체하는 동적 프록시 빈을 만들어 등록 시켜준다.
  - 클라이언트 코드 변경 없음.
  - AbstractAutoProxyCreator implements BeanPostProcessor



# 예시

### EventService.java
```java
public interface EventService {
   void createEvent();
   void publishEvent();

}

```

### SimpleEventService.java
```java
@Service
public class SimpleEventService {

   @Override
   public void createEvent() {
      try {
         Thread.sleep(1000);
      } catch (InterruptedException e) {
         e.printStackTrace();
      }

      System.out.println("Created an event");
   }

   @Override
   public void publishEvent() {
      try{
         Thread.sleep(2000);
      } catch (InterruptedException e) {
         System.out.println("Published an event");
      }
   
   }

}


```
### AppRunner.java
```java
public class AppRunner implements ApplicationRunner {
   @Autowired
   EventService eventService;

   @Override
   public void run(ApplicationArguments args) throws Exception {
      eventService.createEvent(); //primary로 설정된 빈을 가져다 쓰게 됨
      eventService.publishEvent(); //primary로 설정된 빈을 가져다 쓰게 됨


   }
}

```

### ProxySimpleEventService.java (스프링 AOP 적용 시 삭제할 것)
```java
@Primary //같은 타입의 빈이 여러개일 때 우선적으로 가져오도록 함
@Autowired
public class ProxySimpleEventService implements EventService {

   @Autowired
   SimpleEventService simpleEventService;

   @Override
   public void createEvent() {
      long begin = System.currentTimeMillis();
      simpleEventService.createEvent();
      System.out.println(System.currentTimeMillis() - begin);
   } 

   @Override
   public void publishEvent() {
      long begin = System.currentTimeMillis();
      simpleEventService.publishEvent();
      System.out.println(System.currentTimeMillis() - begin);
   }

}

```

### DemospringApplication.java

- web 안 띄우고 빠르게 실횅하는 설정 (테스트할 때)

```java
public class DemospringApplication {
   public static void main(String[] args) {
      SpringApplication app = new SpringApplication(DemospringApplicaiton.class);
      app.setWebApplicationType(WebApplicationType.NONE); //web server 실행 X
      app.run(args);
      SpringApplication.run(DemospringApplication.class, args);
   }
}

```