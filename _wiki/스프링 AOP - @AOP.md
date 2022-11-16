---
layout: wiki
title: 스프링 AOP - @AOP
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

- [스프링 AOP - @AOP](https://www.inflearn.com/course/spring-framework_core/unit/15527)

# 스프링 AOP
- 애노테이션 기반의 스프링 @AOP

## 의존성 추가
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

## Aspect 정의
- @Aspect
- 빈으로 등록해야 하니까 (컴포넌트 스캔을 사용한다면) @Component도 추가.

## Pointcut 정의
- @Pointcut(표현식)
- 주요 표현식
  - execution
  - @annotation
  - bean
- 포인트컷 조합
  - &&, ||, !

## Advice 정의
- @Before
- @AfterReturning
- @AfterThrowing
- @Around

## 참고
https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-pointcuts


# 예시

## execution

### PerfAspect
```java
@Component
@Aspect
public class PerfAspect {

    @Around("execution(*.me.whiteship..*.EventService.*(..))")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        Object retVal = pjp.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return retVal;
    }
}
```
## Anotation 만들어서 적용하기
- @Retention(RetentionPolicy.~)
    - 어노테이션 정보를 얼마나 유지할 것인지.
    - CLASS는 class 파일 까지 유지하겠다는 것임.
    - 컴파일한 클래스 파일에도 이 정보가 남아있게 됨 (바이트코드에도 해당 어노테이션 정보가 있음)
    - 디폴트가 CLASS이므로 이 어노테이션 따로 안붙여도 동일하게 작동.
    - RetentionPolicy.SOURCE : 컴파일하고 나면 사라짐
    - RetentionPolicy.RUNTIME : 런타임까지 유지. 그러나 굳이 런타임까지 유지할 필요는 없음   

### PerLogging.java
```java
@Retention(RetentionPolicy.CLASS)
@Documented //javadoc 만들때 document에 포함될 수 있또록
@Target(ElementType.METHOD)
public @Interface PerLogging {

}
```

### PerfAspect.java
```java

@Component
@Aspect
public class PerfAspect {
    @Around("@Annotation(PerLogging)")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        Object retVal = pjp.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return retVal;
    }
}

```
### SimpleEventService.java
```java
@Service
public class SimpleEventService {

   @PerLogging
   @Override
   public void createEvent() {
      try {
         Thread.sleep(1000);
      } catch (InterruptedException e) {
         e.printStackTrace();
      }

      System.out.println("Created an event");
   }

   @PerLogging
   @Override
   public void publishEvent() {
      try{
         Thread.sleep(2000);
      } catch (InterruptedException e) {
         System.out.println("Published an event");
      }
   
   }

   public void deleteEvent() {
     System.out.println("Delete an event")
   }


}
```
## Bean

### PerfAspect
```java
@Component
@Aspect
public class PerfAspect {

    @Around("bean(simpleEventService)")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        Object retVal = pjp.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return retVal;
    }

    @Before("bean(simpleEventService)")
    public void hello() {
        System.out.println("hello");
    }

}
```



## Intellij는 어디에 적용되는지 알려줌
- 그러나 여기저기 흩어져 있다면 툴에 의존하기 보다는, annotation 만들어서 쓰는 방식이 더 편할 것
- ![image](https://user-images.githubusercontent.com/114462413/202189801-be201132-1c0f-4a45-b475-270e703eb4b2.png)

- ![image](https://user-images.githubusercontent.com/114462413/202189945-dfdeebc1-ae8e-4920-a9f6-da70f750a2c7.png)
