---
layout: wiki
title: IoC 컨테이너 6부 - Environment 1부. 프로파일
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

- [IoC 컨테이너 6부 - Environment 1부. 프로파일](https://www.inflearn.com/course/spring-framework_core/unit/15511?category=questionDetail)

# EnvironmentCapable
- ApplicationContext가 상속받는 인터페이스
- 프로파일과 프로퍼티를 다루는 인터페이스
- getEnvironment()

  ![image](https://user-images.githubusercontent.com/114462413/201141465-25ebf340-308b-4ccb-8d37-9be938e50c48.png)

# 프로파일
- 빈들의 그룹, 묶음
- Environment의 역할은 활성화할 프로파일 확인 및 설정

# 프로파일 useCase
- 테스트 환경에서는 A라는 빈을 사용하고, 배포 환경에서는 B라는 빈을 쓰고 싶다.
- 이 빈은 모니터링 용도니까 테스트할 때는 필요가 없고, 배포할 때만 등록이 되면 좋겠다.

# 프로파일 확인해보기
- 코드
  ```java
  @Component
  public class AppRunner implements ApplicationRunner {
    @Autowired
    Application ctx;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Environment environment = ctx.getEnvironment();
        System.out.println(Arrays.toString(environment.getActiveProfiles()));
        System.out.println(Arrays.toString(environment.getDefaultProfiles()));
    }
  }
  ```

  - 출력
  ```
  []
  [default]
  ```

# 프로파일 정의하기
  ## 1. 클래스에 정의
   ### 1. 직접 configuration 클래스에 등록하기
   -  @Configuration @Profile("test")
    ```java
    @Configuration
    @Profile("test")
    public class TestConfiguration {
        @Bean
        public BookRepository bookRepository() {
            return new TestBookRepository();
        }
    }
    ```
      - 다른 프로파일에서는 해당 빈이 등록되어 있지 않기 때문에, 가져올 수 없다
      ```java
      @Component
      public class AppRunner implements ApplicationRunner {
        @Autowired
        Application ctx;

        @Autowired
        BookRepository bookRepository; // 프로파일이 디폴트이므로, test 프로파일에 등록된 빈을 못찾음
        

        @Override
        public void run(ApplicationArguments args) throws Exception {

        } //실행 시 에러 발생
      }
      ```
 
### 2. 개별 클래스에 프로파일 설정
  
 - @Component @Profile("test")
 - 컴포넌트 스캔을 하면서 적용됨

   ![image](https://user-images.githubusercontent.com/114462413/201154139-d64288c2-61f1-46f3-890c-7db4244bc9bb.png)

## 2. 메소드에 정의
- @Bean @Profile("test")

# 프로파일을 변경하기

 1. IDE에서 지원하는 방식 사용
 ![image](https://user-images.githubusercontent.com/114462413/201150776-92de13a3-8492-42bc-a782-c801cc9c1f70.png)

 1. VM Option에 설정
 ![image](https://user-images.githubusercontent.com/114462413/201151344-23e3e74f-73dd-4542-b322-0a0c5fd0326a.png)

 3. @ActiveProfiles (테스트용)

# 프로파일 표현식
1. ! : not
  ![image](https://user-images.githubusercontent.com/114462413/201155054-5459269f-5393-4757-838f-9163f331ddca.png)
2. & : and

3. | : or