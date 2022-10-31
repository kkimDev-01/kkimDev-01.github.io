---
layout: wiki
title: IoC_컨테이너
summary:
date: 2022-11-01 00:00:00 +0900
updated: 2022-11-01 00:00:00 +0900
tag: Spring
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [IoC (Inversion of Control) 컨테이너](https://www.inflearn.com/course/spring/unit/15539)

## IoC (Inversion of Control) 컨테이너

### ApplicationContext (BeanFactory)

- 빈(bean)을 만들고 엮어주며 제공해준다.
- 빈 설정
  - 이름 또는 ID
  - 타입
  - 스코프
- 아이러니하게도 컨테이너를 직접 쓸 일은 많지 않다.
  - spring-boot로 오면서 기본설정으로 감추어짐
