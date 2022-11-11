---
layout: wiki
title: IoC 컨테이너 7부 - MessageSource
summary:
date: 2022-11-11 00:00:00 +0900
updated: 2022-11-11 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [IoC 컨테이너 7부 - MessageSource](https://www.inflearn.com/course/spring-framework_core/unit/15513)

# MessageSource

## 국제화 (i18) 기능을 제공하는 인터페이스

## ApplicationContext extends MessageSource
 
- ![image](https://user-images.githubusercontent.com/114462413/201352513-8b604aa0-2943-4d82-b92b-b0a38c19d675.png)

- ![image](https://user-images.githubusercontent.com/114462413/201353070-7523c8ca-2ad5-467a-a218-12c63a05a418.png)


## 스프링 부트를 사용한다면, 별다른 설정 필요없이 messages.properties를 사용할 수 있음

### 스프링부트에 자동으로 빈으로 등록되는 ResourceBundleMessageSource가,  messages~로 시작하는 classPath 내의 파일들을 번들로 인식하기 때문
- messages.properties
- messages_ko_kr.properties
- ![image](https://user-images.githubusercontent.com/114462413/201356246-4858590c-8a46-4d15-befc-755f1d6d707b.png)


### 예시
```java
//AppRunner.java

@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    MessageSource messageSource;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.KOREA));
        System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.getDefault()));
    }
}
``` 

```java
//messages.properties
greeting=Hello, {0}

```

```java
// messages_ko_kr.properties
greeting=안녕, {0}

```

```java
//출력결과
Hello, keesun
안녕, keesun
```


## Reloading 기능이 있는 메시지 소스 사용하기

- 메시지가 출력되고 있는 와중에, message properties에 있는 메시지 내용을 바꾸고 build하면 3초라는 캐시 타임이 지나고 나서 다시 해당 내용을 읽고 출력을 하게되므로, 출력되는 메시지 내용이 바뀜 (Reloading)


### 1. MessageSource 빈 커스터마이징
```java 
//DemospringApplication.java

@SpringBootApplication
public class DemospringApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemospringApplication.class, args);
    }

    @Bean
    public MessageSource messageSource() {
        var messageSource = new ReloadableResourceBundleMessageSource();
        messageSource.setBasename("classpath:/message");
        messageSource.setDefaultEncoding("UTF-8");
        messageSource.setCacheSeconds(3); //메시지 리소스를 최대 3초까지만 caching하고, 다시 읽도록 설정함 (Reloading) 
        return messageSource;
    }

}


```

### 2. 1초마다 메시지를 찍도록 한다(확인용).


```java
@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    MessageSource messageSource;

    @Override
    public void run(ApplicationArguments args) throws Exception {

        while(true) {
            System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.KOREA));
            System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.getDefault()));
        }
        Thread.sleep(1000L);
        
    }
}

```