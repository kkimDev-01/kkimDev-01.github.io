---
layout: wiki
title: null-safety
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

- [Null-safety](https://www.inflearn.com/course/spring-framework_core/unit/15529)

# Null-safety
### 스프링 프레임워크 5에 추가된 Null 관련 애노테이션

- @NonNull
  - 파라미터나 리턴이 null일 수 없게 하는 것
  - ![image](https://user-images.githubusercontent.com/114462413/202196758-f662b0f0-b892-43d0-9ee2-1f162ae2b078.png)

- @Nullable
- @NonNullApi (패키지 레벨 설정)
  - 해당 패키지 아래의 모든 파라미터와 리턴 타입에 NonNull을 적용하는 효과
  - 예외적으로 Null을 허용할 때는 @Nullable을 붙여 사용
  - ![image](https://user-images.githubusercontent.com/114462413/202198289-4ddc3f47-1b72-487c-a5b8-bde9191c6047.png)

- @NonNullFields (패키지 레벨 설정)
### 목적
- (툴의 지원을 받아) 컴파일 시점에 최대한 NullPointerException을 방지하는 것

### Intellij 설정 : Compiler 옵션에 Spring의 nullable 및 NonNull 어노테이션 추가 필요
- 컴파일러 옵션에 들어간다.
  - ![image](https://user-images.githubusercontent.com/114462413/202197569-324c9b95-67c1-402a-83cc-33ab852288cc.png)

- Nullable 및 NonNull 스프링 어노테이션을 추가한다
  - ![image](https://user-images.githubusercontent.com/114462413/202197804-4dd3a97c-7927-4475-bb63-b9646f090d0f.png)


