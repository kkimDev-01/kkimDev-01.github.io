---
layout: wiki
title: Validation 추상화
summary:
date: 2022-11-13 00:00:00 +0900
updated: 2022-11-13 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [Validation 추상화](https://www.inflearn.com/course/spring-framework_core/unit/15518)

# Validation 추상화

- org.springframework.validation.Validator

- 어플리케이션에서 사용하는 객체 검증용 인터페이스
  
## 특징
- 어떠한 계층과도 관계가 없다
  - 모든 계층 (웹, 서비스, 데이터)에서 사용해도 좋다.
- 구현체 중 하나로, JSR-303(Bean Validation 1.0)과 JSR-349(Bean Validation 1.1)을 지원한다. (LocalValidatorFactoryBean)
- DataBinder에 들어가 바인딩 할 때 같이 사용되기도 한다.

## 인터페이스
- boolean supports(Class clazz): 어떤 타입의 객체를 검증할 때 사용할 것인지 결정함
- void validate(Object obj, Errors e): 실제 검증 로직을 이 안에서 구현
  - 구현할 때 ValidationUtils 사용하며 편리 함.

## 스프링 부트 2.0.5 이상 버전을 사용할 때
- LocalValidatorFactoryBean 빈으로 자동 등록
- JSR-380(Bean Validation 2.0.1) 구현체로 hibernate-validator 사용.
- https://beanvalidation.org/


# 예시 

## 1 : 스프링부트 이전 (직접 구현)

- Event.java
```java
public class Event {
    Integer id;
    String title;

    //+ Getter, Setter
}
```

- EventValidator.java
```java

public class EventValidator implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "notempty","");
    }


}

```

- AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        Event event = new Event();
        EventValidator eventValidator = new EventValidator();
        Errors errors = new BeanPropertyBindingResult(event, "event");

        eventValidator.validate(event, errors);
        errors.getAllErrors().forEach(e -> {
            System.out.println("===== error code =====");
            Arrays.stream(e.getCodes()).forEach(System.out::println);
            System.out.println(e.getDefaultMessage());
        });
    }
}
```

- 실행결과

    ![image](https://user-images.githubusercontent.com/114462413/201521362-42b8ad25-3b41-4023-8cb8-bac0de24f70c.png)

## 2. 스프링부트 이전 직접구현 : Validator 내부에서 event 다루기

- EventValidator.java
```java

public class EventValidator implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "notempty","");

        Event event = (Event)target;

        if (event.getTitle() == null) {
            //errors.reject(~)
            //errors.rejectValue(~)

        }

    }


}

```

## 3. 스프링부트 2.0.5 이후 : (자동 등록되어 있는) LocalValidatorFactoryBean을 사용

- Event.java
```java
public class Event {
    
    Integer id;

    @NotEmpty
    String title;

    @NotNull @Min(0)
    Integer limit;

    @Email
    String email;

    //+ Getter, Setter
}
```



- AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    Validator validator;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Event event = new Event();
        event.setLimit(-1); //에러나게 이상한 값을 넣음
        event.setEmail("aaa2"); //에러나게 이상한 값을 넣음
        
        // EventValidator eventValidator = new EventValidator(); --> 스프링부트의 빈을 사용할 것이므로 필요 없음
        
        Errors errors = new BeanPropertyBindingResult(event, "event");

        // eventValidator.validate(event, errors);

        validator.validate(event, errors);
        errors.getAllErrors().forEach(e -> {
            System.out.println("===== error code =====");
            Arrays.stream(e.getCodes()).forEach(System.out::println);
            System.out.println(e.getDefaultMessage());
        });
    }
}
```

- 실행결과

![image](https://user-images.githubusercontent.com/114462413/201522081-8cc71533-da25-4340-a1c7-b6157c3be5da.png)
