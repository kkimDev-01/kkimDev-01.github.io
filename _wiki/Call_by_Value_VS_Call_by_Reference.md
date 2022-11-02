---
layout: wiki
title: Call_by_Value_VS_Call_by_Reference
summary:
date: 2022-11-02 00:00:00 +0900
updated: 2022-11-02 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [Java - Call by Value vs Call by Reference](https://gwang920.github.io/java/CallbyValue-CallbyReference/)

## 결론 : JAVA는 항상 Call by Value

## 배열, 객체는 Call by Reference ?

1. JVM 메모리에 변수가 저장되는 위치
   ![image](https://user-images.githubusercontent.com/114462413/199507473-a1256ce9-329f-4d81-855f-4adaf7a5c427.png)

- Java 에서 변수를 선언하면 Stack 영역에 할당된다.
- 원시 타입 (Primitive Type) 은 Stack 영역에 변수와 함께 저장되며
- 참조 타입 (Reference Type) 객체는 Heap 영역에 저장되고 Stack 영역에 있는 변수가 객체의 주소값을 갖고 있다.

2. '레퍼런스에 의한 전달'(Call by Reference)

- 값(심지어 기본값이라고 하더라도) 이 메소드에 바로 전달되지 않는다.
- 대신에, 메소드는 레퍼런스를 값으로 전달받는다.
- 그러므로 메소드가 매개변수를 변경한다면, 이 변경은 메소드가 리턴했을 때 반영 되어 있을 것이며, 매개변수가 기본형이라도 이는 마찬가지이다. (여기서 기본형이란 자바의 primitive type)
- 원시

3. '값에 의한 전달'(Call by Value)

- 자바는 이렇게 하지 않는다. 자바는 '값에 의한 전달' 언어이다.
- 메서드를 호출하는 호출자 (Caller) 의 변수와 호출 당하는 수신자 (Callee) 의 파라미터는 복사된 서로 다른 변수
- 값만을 전달하기 때문에 수신자의 파라미터를 수정해도 호출자의 변수에는 아무런 영향이 없다.
- 레퍼런스 타입의 경우에는 전달되는 값이 레퍼런스이다.
- 이것은 '레퍼런스에 의한 전달'과 다르다.
- '레퍼런스에 의한 전달'이라면, 레퍼런스 타입이 메소드에 전달될 때 레퍼런스에 대한 레퍼런스가 전달되었을 것이다.

4. 배열이나 객체를 함수의 argument로 전달 했을 때 call by reference로 동작하는 것처럼 보였다.
   - argument를 전달받은 메소드에서 배열이나 객체의 값을 변경하면 실제로 값이 변경된다
   - 하지만, 자바는 항상 call by value로 동작한다 : reference type에 한해서 call by reference로 동작하듯 보일 뿐이다.
   - JAVA는 항상 call by value이다. 다만, primitive type 이 아닌 reference type 에 한해서 call by reference 로 동작하는 것처럼 보일 뿐이다.
   - 자바가 '레퍼런스에 의해(by reference)' 배열과 객체를 처리한다고 설명하였다. 이를 '레퍼런스에 의한 전달(pass by reference, Call by Reference)'와 혼동하지 말기를 바란다.

## 원시 타입 (Primitive Type) 전달 : Call by Value

- int 등

  ```java
    public class PrimitiveTypeTest {

        @Test
        @DisplayName("Primitive Type 은 Stack 메모리에 저장되어서 변경해도 원본 변수에 영향이 없다")
        void test() {
            int a = 1;
            int b = 2;

            // Before
            assertEquals(a, 1);
            assertEquals(b, 2);

            modify(a, b);

            // After: modify(a, b) 호출 후에도 값이 변하지 않음
            assertEquals(a, 1);
            assertEquals(b, 2);
        }

        private void modify(int a, int b) {
            // 여기 있는 파라미터 a, b 는 이름만 같을 뿐 test() 에 있는 a, b 와 다른 변수
            a = 5;
            b = 10;
        }
    }
  ```

![image](https://user-images.githubusercontent.com/114462413/199509468-66979551-2006-40b9-9d4a-dc056e4a93d6.png)

- test() 의 변수 a, b 와 modify(a, b) 로 전달받은 파라미터 a, b 의 이름과 값은 같다.

- 하지만 다른 변수이다

- modify(a, b) 를 호출하는 순간 Stack 영역에 새로운 변수 a, b 가 새로 생성되어 총 4 개의 변수가 존재한다.

- Stack 내부에 test() 와 modify() 라는 영역이 나뉘어져 있고 거기에 동일한 이름을 가진 변수 a, b 가 존재한다.

- modify() 영역의 값을 바꿔도 test() 영역의 변수는 변화가 없다.

## 참조 타입 (Reference Type)의 전달

- Object, Array 등

```java
    class User {
        public int age;

        public User(int age) {
            this.age = age;
        }
    }

    public class ReferenceTypeTest {

        @Test
        @DisplayName("Reference Type 은 주소값을 넘겨 받아서 같은 객체를 바라본다" +
                    "그래서 변경하면 원본 변수에도 영향이 있다")
        void test() {
            User a = new User(10);
            User b = new User(20);

            // Before
            assertEquals(a.age, 10);
            assertEquals(b.age, 20);

            modify(a, b);

            // After
            assertEquals(a.age, 11);
            assertEquals(b.age, 20);
        }

        private void modify(User a, User b) {
            // a, b 와 이름이 같고 같은 객체를 바라본다.
            // 하지만 test 에 있는 변수와 확실히 다른 변수다.

            // modify 의 a 와 test 의 a 는 같은 객체를 바라봐서 영향이 있음
            a.age++;

            // b 에 새로운 객체를 할당하면 가리키는 객체가 달라지고 원본에는 영향 없음
            b = new User(30);
            b.age++;
        }
    }

```

### 처음 변수 선언 시 메모리 상태

1. 원시 타입과는 다르게, 변수만 Stack 영역에 생성되고 실제 객체는 Heap 영역에 생성된다.

2. 각 변수는 Heap 영역에 있는 객체를 바라보고 있다.

![image](https://user-images.githubusercontent.com/114462413/199510772-bbf1b200-79bb-4494-95cb-c7879c7aae5c.png)

### modify(a, b) 호출 시점의 메모리 상태

- 넘겨받은 파라미터는 Stack 영역에 생성되고 넘겨받은 주소값을 똑같이 바라본다.

![image](https://user-images.githubusercontent.com/114462413/199510803-92226712-043d-457b-9931-196a26690950.png)

### modify(a, b) 수행 직후 메모리 상태

1. test() 영역과 modify() 영역에 존재하는 a 라는 변수들은 같은 객체인 User01 을 바라보고 있기 때문에 객체를 공유합니다.

2. b 라는 변수는 서로 같은 객체인 User02 를 바라보고 있었지만 modify(a, b) 내부에서 새로운 객체를 생성해서 할당했기 때문에 User03 이라는 객체를 바라봅니다.

3. 그래서 User03 의 age 값을 변경해도 test() 에 있는 b 에는 아무런 변화가 없습니다.

![image](https://user-images.githubusercontent.com/114462413/199510833-62e00e66-9e41-400b-b215-a5b0e5de5c29.png)

### test() 끝난 후 최종 메모리 상태

1. modify(a, b) 메서드를 빠져나오면 Stack 영역에 할당된 변수들은 사라집니다.

2. 최종적으로 위와 같은 상태가 되며 User03 은 어떤 곳에서도 참조되고 있지 않기 때문에 나중에 Garbage Collector 에 의해 제거될 겁니다.

![image](https://user-images.githubusercontent.com/114462413/199510888-febe8136-f3af-4410-9476-f4b3a9d72bb8.png)

### 결론

- 호출자 변수와 수신자 파라미터는 Stack 영역 내에서 각각 존재하는 다른 변수다.
