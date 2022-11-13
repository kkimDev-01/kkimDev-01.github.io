---
layout: wiki
title: 데이터 바인딩 추상화 - Converter와 Formatter
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

- [데이터 바인딩 추상화 - Converter와 Formatter](https://www.inflearn.com/course/spring-framework_core/unit/15521)

# Converter
- S 타입을 T 타입으로 변환할 수 있는 매우 일반적인 변환기
- 상태 정보 없음
  - == Stateless
  - == Thread-safe (쓰레드-세이프하다)
- ConverterRegistry에 등록해서 사용
  - @Configuration 자바파일에서 WebConversionService의 addConverter를 사용하면 등록됨
  - addConverter 메서드 -> ConversionService에 등록 -> ConversionService에서 실제 변환 작업

# Formatter
- PropertyEditor의 대체재
- Object와 String 간의 변환을 담당한다.
- 문자열을 Locale에 따라 다국화하는 기능도 제공한다 (optional)
- FormatterRegistry에 등록해서 사용
  - @Configuration 자바파일에서 WebConversionService의 addFormatter를 사용하면 등록
  - addFormatter를 메서드 -> ConversionService에 등록 -> ConversionService에서 실제 변환 작업

# ConversionService
- 실제 변환 작업은 이 인터페이스를 통해 쓰레드-세이프하게 사용할 수 있음
- 스프링 MVC, 빈(value) 설정, SpEL에서 사용함
- DefaultFormattingConversionService 클래스가 주로 사용됨
  - FormatterRegistry 기능도 하고 (이 인터페이스를 ConversionService가 구현했음)
  - ConversionService 기능도 함 (이 인터페이스를 ConversionService가 구현했음)
  - 여러 기본 Converter와 Formatter를 등록해줌.

# 구조
![image](https://user-images.githubusercontent.com/114462413/201526528-396f7af8-a32f-4d15-b345-336a59052287.png)


- ConverterRegistry는 FormatterRegistry를 상속받고 있음

- 스프링 부트
  - 웹 애플리케이션인 경우에 DefaultFormattingConversionSerivce를 상속하여 만든 WebConversionService를 빈으로 등록해 준다.
    - ![image](https://user-images.githubusercontent.com/114462413/201526508-a70f0421-7f69-4d53-b43d-77a8cd7f7875.png)
  - Formatter와 Converter 빈을 찾아 자동으로 등록해 준다.
    - Formatter와 Converter는 쓰레드-세이프하므로, 빈으로 등록해서 사용해도 된다.
    - ![image](https://user-images.githubusercontent.com/114462413/201526712-5e8e13e5-b832-47f0-9bad-24b0f659ae52.png)
    - ![image](https://user-images.githubusercontent.com/114462413/201526807-0af8310d-e352-4908-a1a1-91634abb6024.png)
  - 테스트
    - @WebMvcTest를 붙이면, Web에 필요한 빈만 가져온다. (주로 Controller들)
    - 가져올 클래스를 명시해주면 해당 클래스 빈들도 가져온다.
    - ![image](https://user-images.githubusercontent.com/114462413/201526923-e6096b17-cfe0-4476-b967-40c661dab006.png)
    - ![image](https://user-images.githubusercontent.com/114462413/201526977-ff8a9f15-17ed-4bda-a0e1-6be8e8a3b280.png)



# 예시

## Converter

### EventConverter.java

```java
public class EventConverter {
    public static class StringToEventConverter implements Converter<String, Event> {
        @Override
        public Event convert(String source) {
            Event event = new Event();
            event.setId(Integer.parseInt(source));
            return event;
        }
    }

        public static class EventToString implements Converter<Event, String> {
        @Override
        public String convert(Event source) {
            return source.getId().toString();
        }
    }

}
```

### WebConfig.java
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new EventConverter.StringToEventConverter());
    }
}
```

### EventControllerTest.java
```java
@RunWith(SpringRunner.class)
@WebMvcTest
public class EventControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void getTest() {
        mockMvc.perform(get("/event/1"))
                .andExpect(status.isOk())
                .andExpect(content().string("1"));
    }
}
```

## Formatter

### EventFormatter.java
```java
public class EventFormatter implements Formatter<Event> {

    @Override
    public Event parse(String text, Locale locale) throws ParseException {
        Event event = new Event();
        int id = Integer.parseInt(text);
        event.setId(id);
        return event;
    }

    @Override
    public String print(Event object, Locale locale) {


         return object.getId().toString();
    }
}

```

### WebConfig.java
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new EventFormatter());
    }
}
```


# 첨언
- 일반적인 경우 : Formatter 사용을 추천함
- JPA를 사용한다면 : @Entity 안에 이미 Converter가 있음
- 앱에서 쓰이고 있는 Converter, Formatter 목록 보려면 : conversionService를 출력하면 됨
  - ![image](https://user-images.githubusercontent.com/114462413/201527131-dce9344d-2e37-481b-afbd-acb90b6727fd.png)
