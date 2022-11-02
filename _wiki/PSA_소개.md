---
layout: wiki
title: PSA_소개
summary:
date: 2022-11-03 00:00:00 +0900
updated: 2022-11-03 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [PSA\_소개](https://www.inflearn.com/course/spring/unit/15546)

## PSA = 잘 만든 인터페이스 = 추상화

- 나의 코드

  - 확장성이 좋지 못한 코드 or 기술에 특화되어 있는 코드

- 잘 만든 인터페이스 (PSA)

## 예시1 : 스프링 트랜잭션

- @Transactional
  - 이 어노테이션을 처리하는 Aspect가 있음
  - 그 Aspect 안에는, PlatformTransactionManager 가 있음
  - 기술 독립적
  - 추상화 : 어떻게 구현하던 이 코드는 바뀌지 않는다.
- 구체적 기술 : 구현체
  - JpaTransacionManager | DatasourceTransactionManager | HibernateTransactionManager

## 예시2 : 스프링 캐시

- @EnableCaching
  - @Cacheable | @CacheEvict | ... 사용
  - 이 어노테이션을 처리하는 Aspect가 있음
  - 그 Aspect 안에는, CacheManager 가 있음
  - 기술 독립적
  - 추상화 : 어떻게 구현하던 이 코드는 바뀌지 않는다.
- 구체적 기술 : 구현체
  - JCacheManager | ConcurrentMapCacheManager | EhCacheCacheManager | ...

## 예시3 : 스프링 웹 MVC

- @Controller 와 @RequestMapping
- 구체적 기술 : 구현체
  - Servlet | Reactive
