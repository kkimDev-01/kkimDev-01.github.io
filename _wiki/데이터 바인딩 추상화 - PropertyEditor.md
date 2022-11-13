---
layout: wiki
title: 데이터 바인딩 추상화 - PropertyEditor
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

- [데이터 바인딩 추상화 - PropertyEditor](https://www.inflearn.com/course/spring-framework_core/unit/15520)

# 데이터 바인딩 추상화 : PropertyEditor

- org.springframework.validation.DataBinder

- 기술적인 관점: 프로퍼티 값을 타겟 객체에 설정하는 기능

- 사용자 관점: 사용자 입력값을 애플리케이션 도메인 모델에 동적으로 변환해 넣어주는 기능.

- 해석하자면: 입력값은 대부분 “문자열”인데, 그 값을 객체가 가지고 있는 int, long, Boolean, Date 등 심지어 Event, Book 같은 도메인 타입으로도 변환해서 넣어주는 기능.

## PropertyEditor
- 스프링 3.0 이전까지 DataBinder가 변환 작업 사용하던 인터페이스
- 쓰레드-세이프 하지 않음 (Thread-unsafe)
  - 상태 정보 저장 하고 있음 : 서로 다른 쓰레드가 공유함
  - 따라서 싱글톤 빈으로 등록해서 쓰면 안됨
    - 회원2가 회원1 정보 수정하는 등 상황이 발생
    - Thread Scope의 빈으로 등록하면 사용할 순 있음 : 한 쓰레드 내에서만 유효한 scope의 빈
    - 그러나 웬만하면 빈으로 등록해서 쓰지 않는다. 그게 안전하다.
- 그렇다면 어떻게 사용하는가 : @InitBinder 로 특정 컨트롤러에 묶어서 사용
 
- Object와 String 간의 변환만 할 수 있어, 사용 범위가 제한적임.
  - 그래도 그런 경우가 대부분이기 때문에 잘 사용해 왔음.


# 예시

### Event.java
```java
public class Event {
    Integer id;
    String title;

    //+ Getter, Setter
}
```

### EventController.java
```java
public class EventController {

    @InitBinder
    public void init(WebDataBinder webDataBinder) {
        webDataBinder.registerCustomEditor(Event.class, new EventEditor());
    }

    @GetMapping("/event/{event}")
    public String getEvent(@PathVariable Event event){
        System.out.println(event);
        return event.getId().toString();
    }
}
```
### EventEditor.java
```java
public class EventEditor extends ProperyEditorSupport {

    @Override
    public String getAsText() {
        Event event = (Event) getValue();
        return event.getId().toString();
    }

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        setValue(new Event(Integer.parseInt(text)));
        
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