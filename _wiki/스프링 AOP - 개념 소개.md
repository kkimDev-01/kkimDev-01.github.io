---
layout: wiki
title: 스프링 AOP - 개념 소개
summary:
date: 2022-11-16 00:00:00 +0900
updated: 2022-11-16 00:00:00 +0900
tag: 
toc: true
public: true
parent: [[]]
latex: true
---

# 참고문헌

- [스프링 AOP - 개념 소개](https://www.inflearn.com/course/spring-framework_core/unit/15525)

# 스프링 AOP - 개념 소개

- Aspect-oriendted Programming (AOP)은 OOP를 보완하는 수단으로, 흩어진 Aspect를 모듈화 할 수 있는 프로그래밍 기법

![image](https://user-images.githubusercontent.com/114462413/201966476-49a8916a-6b4e-41af-bc9e-6cf495d178ad.png)

## AOP 주요 개념
### Aspect와 Target
- Aspect : 각각의 모듈
- Target : Aspect가 가지고 있는 Advice가 적용되는 대상들
### Advice
- Advice : 해야할 일
### Join point와 Pointcut
- Joint point : 합류점, 끼어들 수 있는 지점 (매서드 실행 시, 생성자 실행 시, 필드에서 값 가져갈 때 등)
- Pointcut : 어디에 적용되어야 되는지

## AOP 구현체
-  https://en.wikipedia.org/wiki/Aspect-oriented_programming
-  자바
   -  AspectJ
   -  스프링 AOP
   -  
## AOP 적용 방법
-  컴파일
-  로드 타임
-  런타임
   -  스프링 AOP가 사용하는 방식
