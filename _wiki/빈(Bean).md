---
layout: wiki
title: 빈(Bean)
summary:
date: 2022-11-01 00:00:00 +0900
updated: 2022-11-01 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [빈(Bean)](https://www.inflearn.com/course/spring/unit/15540)

## 빈(Bean)

### 스프링 IoC 컨테이너가 관리하는 객체

### 어떻게 등록하지?

1. Component Scanning

- @Component
  - @Repository
  - @Service
  - @Controller

2. 또는 일일이 XML이나 자바설정파일에 등록

### 어떻게 꺼내 쓰지?

1. @Autowired
2. @Inject
3. 또는 ApplicationContext.getBean()으로 직접 꺼내거나
