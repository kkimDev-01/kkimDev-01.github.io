---
layout: wiki
title: IoC 컨테이너 6부 - Environment 2부. 프로퍼티
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

- [IoC 컨테이너 6부 - Environment 2부. 프로퍼티](https://www.inflearn.com/course/spring-framework_core/unit/15512?category=questionDetail)

# 프로퍼티 (Property)

- 다양한 방법으로 정의할 수 있는 설정값
- Environment의 역할은 프로퍼티 소스 설정 및 프로퍼티 값 가져오기

# 프로퍼티 가져오는 방법
1. VM Option
   ![image](https://user-images.githubusercontent.com/114462413/201166118-d3e61277-97f2-40aa-9703-e89e6ba0b4ca.png)

   ![image](https://user-images.githubusercontent.com/114462413/201165880-6207889e-27f8-488c-8f17-f438608d4030.png)

2. .properties 파일
   ![image](https://user-images.githubusercontent.com/114462413/201166634-d0b217fb-836d-49d9-9e87-35e90964adab.png) 
   ![image](https://user-images.githubusercontent.com/114462413/201166825-c59ab8e3-61e3-4e09-8f90-506875ffd69d.png)
   ![image](https://user-images.githubusercontent.com/114462413/201166923-bfe82bde-d24d-4b60-873a-3d3d7c4c9639.png)

   - @Value 로 변수로 가져올 수 있음
   ![image](https://user-images.githubusercontent.com/114462413/201168067-8e3c4e9e-95d9-476b-92cf-bb4cda552167.png)
   

# 프로퍼티의 우선순위 
- StandardServletEnvironment의 우선순위
  - ServletConfig 매개변수
  - ServletContext 매개변수
  - JNDI (java:comp/env/)
  - JVM 시스템 프로퍼티 (-Dkey="value")
  - JVM 시스템 환경 변수 (운영체제 환경 변수)

- VM Option VS .properties 파일 : VM Option이 이김

- @PropertySource
  - Environment를 통해 프로퍼티를 추가하는 방법
  
# 스프링 부트의 외부 설정 참고
- 기본 프로퍼티 소스 지원 : application.properties 
- 프로파일까지 고려한 계층형 프로퍼티 우선 순위 제공
