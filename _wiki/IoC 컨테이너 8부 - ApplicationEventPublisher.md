---
layout: wiki
title: IoC 컨테이너 8부 - ApplicationEventPublisher
summary:
date: 2022-11-12 00:00:00 +0900
updated: 2022-11-12 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [IoC 컨테이너 8부 - ApplicationEventPublisher](https://www.inflearn.com/course/spring-framework_core/unit/15514)


# ApplicationEventPublisher

- 이벤트 프로그래밍에 필요한 인터페이스 제공
- 옵저버 패턴으로 구현한 구현체

- ApplicationContext extends ApplicationEventPublisher
  - publishEvent(ApplicationEvent event)


# 이벤트 발생시키는 방법
- ApplicationEventPublisher.publishEvent();

# 이벤트 처리하는 방법
- ApplicationListener<이벤트> 구현한 클래스 만들어서 빈으로 등록하기
- 스프링 4.2부터는 @EventListener를 사용해서 빈의 메소드에 사용할 수 있다.
- 기본적으로는 synchronized
- 순서를 정하고 싶다면, @Order 와 함께 사용
- 비동기적으로 실행하고 싶다면 @Async 와 함께 사용

# 스프링이 제공하는 기본 이벤트
### ContextRefreshedEvent
- ApplicationContext를 초기화 했더나 리프래시 했을 때 발생.
### ContextStartedEvent 
- ApplicationContext를 start()하여 라이프사이클 빈들이 시작 신호를 받은 시점에 발생.
### ContextStoppedEvent 
- ApplicationContext를 stop()하여 라이프사이클 빈들이 정지 신호를 받은 시점에 발생.
### ContextClosedEvent 
- ApplicationContext를 close()하여 싱글톤 빈 소멸되는 시점에 발생.
### RequestHandledEvent
- HTTP 요청을 처리했을 때 발생.



# 예시 1 :스프링부트 4.2 이전

- MyEvent.java
```java
public class MyEvent extends ApplicationContext {
    private int data;
    
    public MyEvent(Object source) {
        super(source);
    }

    public MyEvent(Object source, int data) {
        super(source);
        this.data = data;
    }

    public int getData() {
        return data; 
    }
}
```

- AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        applicationContext.publishEvent(new MyEvent(this, 100));
    }
```

- MyEventHandler.java

```java
@Component
public class MyEventHandler implements ApplicationListener<MyEvent> {
    @Override
    public void onApplicationEvent(MyEvent event) {
        System.out.println("이벤트 받았다. 데이터는 " + event.getData());
    }
}
```

# 예시 2: 스프링부트 4.2 이후
- 스프링의 클래스들을 implement하지 않아도 됨
- 


- MyEvent.java
```java
public class MyEvent {
    private int data;
    
    private Object source;

    public MyEvent(Object source, int data) {
        this.source = source;
        this.data = data;
    }

    public Object getSource() {
        return sourfce;
    }

    public int getData() {
        return data; 
    }
}
```

- AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        applicationContext.publishEvent(new MyEvent(this, 100));
    }
```

- MyEventHandler.java

```java
@Component
public class MyEventHandler  {
    @EventListnener
    @Order(Ordered.HIGHEST_PRECEDENECE) // 자장 높은 우선순위를 부여. 가장 먼저 실행되게 됨
    @Async //비동기 실행. DemospringApplication (루트 메인 자바클래스)에 @EnableAsynce를 붙여야 제대로 비동기로 작동함
    public void handle(MyEvent event) {
        System.out.println(Thread.currentThread().toString());

        System.out.println("이벤트 받았다. 데이터는 " + event.getData());
    }
}
```
```java
@Component
public class AnotherHandler  {
    @EventListnener 
    @Order(Ordered.HIGHEST_PRECEDENECE + 2) // 자장 높은 우선순위를 부여. 가장 먼저 실행되게 됨
    public void handle(MyEvent event) {
        System.out.println(Thread.currentThread().toString());

        System.out.println("Another" + event.getData());
    }
}

```

# 예시 3 : 스프링이 제공하는 기본 이벤트들
- 하나의 자바파일에 여러개의 이벤트 핸들러를 설정해도 됨
- MyEventHandler.java
```java
@Component
public class MyEventHandler  {
    @EventListnener
    @Async 
    public void handle(MyEvent event) {
        System.out.println(Thread.currentThread().toString());

        System.out.println(event.getData());
    }

    @EventListnener
    @Async 
    public void handle(ContextRefreshedEvent event) {
        System.out.println(Thread.currentThread().toString());

        System.out.println("ContextRefreshedEvent");
    }

    @EventListnener
    @Async 
    public void handle(ContextClosedEvent event) {
        System.out.println(Thread.currentThread().toString());

        System.out.println("ContextClosedEvent"); 
    }
}
```

- 결과 : 시작할 때, 종료될 때 이벤트들이 잘 처리됨
  ![image](https://user-images.githubusercontent.com/114462413/201469031-78856bb9-c645-42fb-8402-ceb016317ea4.png)