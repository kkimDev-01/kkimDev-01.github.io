---
layout: wiki
title: IoC 컨테이너 9부 - ResourceLoader
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

- [IoC 컨테이너 9부 - ResourceLoader](https://www.inflearn.com/course/spring-framework_core/unit/15515)

# ResourceLoader
- 리소스를 읽어오는 기능을 제공하는 인터페이스
- ApplicationContext extends ResourceLoader
- Resource getResource(java.lang.String location)

# Resource 읽어오기
- 파일 시스템에서 읽어오기
- classpath에서 읽어오기
- URL로 읽어오기
- 상대/절대 경로로 읽어오기

# 예시
- AppRunner.java
```java
@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    ResourceLoader resourceLoader;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Resource resource = resourceLoader.getResource("classpath:text.txt");
        System.out.println(resource.exists());
        System.out.println(resource.getDescription());
        System.out.println(Files.readString(Path.of(resource.getURI()))); //Java11부터 사용 가능
    }
}
```

- text.txt
```
hello spring
```

- 결과
```
true
class path resource [text.txt]
hello spring
```