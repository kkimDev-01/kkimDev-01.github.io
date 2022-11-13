---
layout: wiki
title: Resource 추상화
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

- [Resource 추상화](https://www.inflearn.com/course/spring-framework_core/unit/15517)

# org.springframework.core.io.Resource

## 특징
- java.net.URL을 추상화한 것.
- 스프링 내부에서 많이 사용하는 인터페이스

## 추상화한 이유
-  classpath 기준으로 리소스를 읽어오는 기능 부재
-  ServletContext를 기준으로 상대 경로를 읽어오는 기능 부재
-  새로운 핸들러를 등록하여 특별한 URL 접미사를 만들어 사용할 수는 있지만 구현이 복잡하고 편의성 메소드가 부족하다.

## 인터페이스 둘러보기
- Resource 클래스를 상속 받은 인터페이스
  - ClassPathXmlApplicationContext
    - ![image](https://user-images.githubusercontent.com/114462413/201520043-25a1d773-607b-4cd3-90ff-2a8b5da1ff7f.png)\
  - FileSystemXmlApplicationContext
    - ![image](https://user-images.githubusercontent.com/114462413/201520084-732c6403-34ac-4541-8cad-f9f659063ec1.png)
- 주요 메서드
  - getInputStream()
  - exist()
  - isOpen()
  - getDescription() 
    - 전체 경로 포함한 파일이름 또는 실제 URL

## 구현체
- UrlResource
  - java.net.URL 참고
  - 기본으로 지원하는 프로토콜 : http, https, ftp, file, jar
-  ClassPathResource
   -  지원하는 접두어 classpath:
- FileSystemResource
- ServletContextResource
  - 웹 애플리케이션 루트에서 상대 경로로 리소스 찾는다.

## 리소스 읽어오기
- Resource의 타입은 locaion 문자열과 **ApplicationContext의 타입**에 따라 결정 된다.
  - ClassPathXmlApplicationContext -> ClassPathResource
  - FileSystemXmlApplicationContext -> FileSystemResource
  - WebApplicationContext -> ServletContextResource
- ApplicationContext의 타입에 상관없이 리소스 타입을 강제하려면 java.net.URL 접두어(+ **classpath:**)중 하나를 사용할 수 있다.
  - **classpath:**me/whiteship/config.xml -> ClassPathResource
  - **file:**///some/resource/path/config.xml -> FileSystemResource
- classpath: 접두어 없이 사용하면 ServletContextResource으로 resolving이 된다 : "classpath:" 접두어를 명시해서 사용하자
- ![image](https://user-images.githubusercontent.com/114462413/201520488-a61bc52d-2f22-49bf-81f7-2cb8271863aa.png)
