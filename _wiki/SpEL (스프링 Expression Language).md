---
layout: wiki
title: SpEL (스프링 Expression Language)
summary:
date: 2022-11-14 00:00:00 +0900
updated: 2022-11-14 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [SpEL (스프링 Expression Language)](https://www.inflearn.com/course/spring-framework_core/unit/15523)


# SpEL (스프링 Expression Language)

## 스프링 EL이란?
- 객체 그래프를 조회하고 조작하는 기능을 제공한다.
- Unified EL과 비슷하지만, 메소드 호출을 지원하며, 문자열 템플릿 기능도 제공한다.
- OGNL, MVEL, JBOss EL 등 자바에서 사용할 수 있는 여러 EL이 있지만, SpEL은 모든 스프링 프로젝트 전반에 걸쳐 사용할 EL로 만들었다.
- 스프링 3.0 부터 지원.

## SpEL 구성
- ExpressionParser parser = new SpelExpressionParser()
- StandardEvaluationContext context = new StandardEvaluationContext(bean)
- Expression expression = parser.parseExpression(“SpEL 표현식”)
- String value = expression.getvalue(context, String.class)

## 문법
- #{“표현식"}
- ${“프로퍼티"}
- 표현식은 프로퍼티를 가질 수 있지만, 반대는 안 됨.
  - #{${my.data} + 1}
- [레퍼런스 참고](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#expressions-language-ref)


## 실제로 어디서 쓰나?
- @Value 애노테이션
- @ConditionalOnExpression 애노테이션
- [스프링 시큐리티](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/el-access.html)
    - 메소드 시큐리티, @PreAuthorize, @PostAuthorize, @PreFilter, @PostFilter
    - XML 인터셉터 URL 설정
    - ...
- [스프링 데이터](https://spring.io/blog/2014/07/15/spel-support-in-spring-data-jpa-query-definitions)
  - @Query 애노테이션
- [Thymeleaf](https://blog.outsider.ne.kr/997)



# 예시

## AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {

    @Value("#{1+1}")
    int value;

    @Value("#{'hello' + 'world'}")
    String greeting;

    @Value("#{1 eq 1}")
    boolean trueOrFalse;

    @Value("hello") //값 그대로 쓸수도 있음
    String hello;

    @Value("${my.value}") //$ : 프로퍼티를 참고하는 방법
    int myValue;

    @Value("#{${my.value} eq 100}") //프로퍼티를 표현식으로 감싸서 사용 가능 (표현식 안에 프로퍼티 넣기)
    int myValue;

    @Value("#{sample.data}") //sample이라는 빈의 data값을 가져올 수 있음
    int sampleData;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("==========");
        System.out.println(value);
        System.out.println(greeting);
        System.out.println(trueOrFalse);
        System.out.println(hello);
        System.out.println(myValue);

        ExpressionParser parser = new SpelExpressionParser();
        Expression expression = parser.parseExpression("2+100");
        Integer value = expression.getValue(Integer.class);
        System.out.println(value);  //102
    }

}

```

## application.properties
```java
my.value = 100

```

## Sample.java
```java
@Component
public static Sample {
    private int data = 200;

    //Getter & Setter

}

```