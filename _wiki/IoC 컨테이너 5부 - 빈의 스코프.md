---
layout: wiki
title: IoC 컨테이너 5부 - 빈의 스코프
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

- [IoC 컨테이너 5부 - 빈의 스코프](https://www.inflearn.com/course/spring-framework_core/unit/15510?category=questionDetail)


# 스코프 (Scope)

1. 싱글톤
- 빈의 기본 스코프 (디폴트)
- 이 애플리케이션 전반에 걸쳐서 해당 빈의 인스턴스가 오직 한개뿐이라는 것
- 대부분의 경우는 싱글톤 스코프를 쓰게 됨

2. 프로토타입
- Request
- Session
- WebSocket

- 예시
  ![image](https://user-images.githubusercontent.com/114462413/201132390-e34be7dc-a3de-44ec-b0d7-e69ee2284bef.png)
  ![image](https://user-images.githubusercontent.com/114462413/201132771-b6dd93c8-d2c6-4e3f-b4be-d5949a21d03f.png)
  
# 프로토타입 빈이 싱글톤 빈을 참조하면?
- 아무 문제 없음 : 의도한 대로, 모든 프로토타입 빈들이 동일한 싱글톤 빈을 사용하게 됨
![image](https://user-images.githubusercontent.com/114462413/201133551-13639332-b603-4ae5-9576-3fe2e9813512.png)


# 싱글톤 빈이 프로토타입 빈을 참조하면?
- 프로토타입이 업데이트가 안됨 : 싱글톤 빈을 사용할 때마다 그 안에서 참조되고 있는 프로토타입 빈 역시 계속 동일한 빈을 참조함

  ![image](https://user-images.githubusercontent.com/114462413/201134125-ef9dd92f-c276-47b0-b918-81093aaafa27.png)

  ![image](https://user-images.githubusercontent.com/114462413/201134359-47cf2c66-7923-44f2-ae45-6d104f8a8852.png)

- 업데이트하려면? (의도한대로, 싱글톤 빈이 사용될 때마다 새로운 프로토타입 빈 인스턴스를 참조하게 하려면)
    ### 1. scoped-proxy
    ![image](https://user-images.githubusercontent.com/114462413/201137249-5213eb3e-166f-427d-b64b-796e1c1da977.png)
    - 프로토타입 빈을 프록시 빈으로 감싸서, 싱글톤 빈이 프로토타입 빈을 직접 참조하는 게 아니라 프록시 빈을 사용하도록 한다.
    - 이 프록시 빈도 프로토타입 빈을 상속해서 만들어졌기 때문에 타입이 같아서 프로토타입 빈의 형태로 주입이 가능함
    - 
   
    ```java
    @Component @Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class Proto {

    }
    ```
    ![image](https://user-images.githubusercontent.com/114462413/201136005-fdffdf37-9b46-487f-b88a-6fdbbb4e5c9f.png)


    ### 2. Object-Provider

    ![image](https://user-images.githubusercontent.com/114462413/201138030-a6c7e3a3-cc70-4d7b-9503-527b10119cb4.png)

    ### 3. Provider (표준)


# 싱글톤 객체 사용시 주의할 점
1. 프로퍼티가 공유된다.
  ![image](https://user-images.githubusercontent.com/114462413/201138753-f8514d0a-744d-4906-ac36-9c15ac3b283d.png)

  - 이 싱글톤 빈을 여러군데에서 쓰면서 value를 수정할 경우
    - value의 값이 안정적일 것이라고, thread-safe할 것이라고 단정할 수 없다.
    - 예컨대, A라는 thread에서 value = 1이었는데, B라는 thread에서 동시에 value를 2로 수정한 경우, A라는 thread에서 value를 출력했을 때 2가 나올 수 있음.
  - thread-safe 한 방법으로 코딩을 해야한다.
2. 싱글톤 빈들은 ApplicationContext 초기 구동 시 인스턴스가 생성됨
  - 처음 구동할 때 시간이 걸릴 수 있다.