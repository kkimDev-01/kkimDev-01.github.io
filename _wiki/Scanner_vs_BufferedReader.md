---
layout: wiki
title: Scanner_vs_BufferedReader
summary:
date: 2022-- 00:00:00 +0900
updated: 2022-- 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [자바(JAVA) - Scanner & BufferedReader](https://dlee0129.tistory.com/238)
- [JAVA [자바] - 입력 뜯어보기 [Scanner, InputStream, BufferedReader]](https://st-lab.tistory.com/41)

## 공통

- System.in을 입력으로 받을 수 있다
- InputStreamReader를 생성하여 매개변수로 전달받을 수 있다.

```java
Scanner sc = new Scanner(new InputStreamReader(System.in));

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

```

## 가장 큰 차이 : 속도

- 평균적으로 BufferedReader 사용 시 0.6585초, Scanner 사용 시 4.8448초가 걸린다.
  - 데이터가 많아질수록 성능 차이는 커진다.
- 차이가 나는 이유
  1. Buffer의 사용 여부
     - Scanner는 1KB 크기의 버퍼를 가지므로 입력이 바로 전달된다.
     - BufferedReader는 8KB 크기의 버퍼를 가지며, buffer에 입력들을 저장하였다가 한 번에 전송하기 때문에 속도가 더 빠르다
  2. 입력을 읽는 방식
     - Scanner는 입력을 읽는 과정에서 정규표현식 적용, 입력값 분할, 파싱 과정 등을 거치기 때문에 속도가 느리다.

## 두번째 차이 : 예외처리 방식

- Scanner는 System.in을 생성 시에 내부에서 try-catch를 사용하여 예외처리를 하기 때문에 따로 예외처리를 하지 않아도 된다.
- BufferedReader는 예외처리를 해야 한다.

---

## 보다 엄밀하게

---

## 요약

1. InputStream 은 바이트 단위로 데이터를 처리한다. 또한 System.in 의 타입도 InputStream 이다.
2. InputStreamReader 은 문자(character) 단위로 데이터를 처리할 수 있도록 돕는다. InputStream 의 데이터를 문자로 변환하는 중개 역할을 한다.
3. BufferedReader 은 스트림에 버퍼를 두어 문자를 버퍼에 일정 정도 저장해둔 뒤 한 번에 보낸다.

## 자바의 인코딩 방식

- 입력(UTF-8) -> 송수신(modified UTF-8) -> 자바 메모리 (UTF-16) -> 송수신(modified UTF-8) -> 출력(UTF-8)
- UTF-8
  - 영어 : 1 Byte
  - 한글 : 3 Byte
- UTF-16
  - 영어 : 2 Byte
  - 한글 : 2 Byte

## System.in 그리고 InputStream

### 스트림(Stream)이란

- "출발지와 도착지를 이어주는 빨대"
- 한 곳에서 다른 곳으로의 데이터 흐름
- 비유 : 고속도로
  - 고속도로에서 역주행을 할 수 없듯 스트림은 단방향으로만 흐르며
  - 고속도로에서는 중앙분리대 혹은 도로 자체가 분리되어 상향행 하향행이 존재하듯 입력스트림과 출력스트림 또한 분리되어 있다.

### System.in & InputStream의 관계

- System.in은 InputStream의 필드이다.

```java
    public static final InputStream in = null;
```

- in 이라는 변수는 InputStream의 변수로 결국 InputStream 타입의 새 변수를 선언하고 그 변수에 System.in 을 할당시킬 수도 있다는 뜻

```java
    InputStream inputStream = System.in;
```

### java.io 패키지는 IOException이라는 예외를 던지기 때문에, 이를 처리해주어야 컴파일된다.

- 예외 : Scanner (java.util), System.out.print

### InputStream은 바이트 스트림이다

1. UTF-8 로 입력을 받는다
2. read() 메소드는 1 byte 만 읽기 때문에 나머지 byte 는 스트림에 잔존하게 된다.
3. 읽어들인 byte 값은 메모리에 UTF-16 에 대응되는 문자의 인코딩방식으로 2진수 값이 저장한다.
4. 출력시 메모리에 저장되어있던 2진수에 대응되는 문자가 UTF-8 로 변환되어 출력된다.

## Scanner(System.in) 그리고 InputStreamReader(System.in)

- Scanner 생성자는 System.in을 입력받아 InputStreamReader를 호출한다.

### InputStreamReader : 문자 스트림

1. 바이트 단위 데이터를 문자(character) 단위 데이터로 처리할 수 있도록 변환해준다.
2. char 배열로 데이터를 받을 수 있다.

### Scanner의 생성자와 메소드 nextInt()의 과정

1. InputStream (바이트스트림) 을 통해 입력 받음
2. 문자로 온전하게 받기 위해 중개자 역할을 하는 InputStreamReader(문자스트림) 을 통해 char 타입으로 데이터를 처리함
3. 입력받은 문자는 입력 메소드( next(), nextInt() 등등.. ) 의 타입에 맞게 정규식을 검사함
4. 정규식 문자열을 Pattern.compile() 이라는 메소드를 통해 Pattern 타입으로 변환함
5. 반환된 Pattern 타입을 String으로 변환함
6. String 은 입력 메소드의 타입에 맞게 반환함 ( nextInt() - Integer.parseInt() / nextDouble() - Double.parseDouble() 등등.. )

## BufferedReader 그리고 InputStreamReader(System.in)

### BufferedReader의 특징

- 선언

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
```

- 문자열을 처리할 수 있다
  - 앞서 Scanner 에서 InputStreamReader 을 설명할 때 '문자'를 처리한다고 했다. '문자열'이 아니다.
  - InputStreamReader 로 char type 으로 처리할 수 있는 장점은 개선되었는데, 만약 문자열을 입력하고 싶다면 매번 배열을 선언해야 한다는 단점은 그대로 남아있다. 심지어 입력받을 문자열의 길이가 가변적이라면 더욱 불편하다.
  - 그래서 쓰는 것이 Buffer(버퍼)를 통해 입력받은 문자를 쌓아둔 뒤 한 번에 문자열처럼 보내버리는 것이다.

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
```

- System.in = InputStream -> InputStreamReader -> BufferedReader
  - byte 타입으로 읽어들이는 in을 char 타입으로 처리한 뒤 String, 즉 문자열로 저장할 수 있게 한다

### BufferedReader가 Scanner보다 빠른 이유

1. BufferedReader는 버퍼가 있는 스트림이다.
   - 버퍼를 따로 설정하지 않으면, 디폴트로 8192개의 문자를 저장할 수 있다.
   - 개행이 입력되거나 버퍼가 꽉 차게 되면 버퍼를 비우면서 프로그램으로 데이터를 보낸다.
   - 하나하나 문자를 보내는 것이 아닌 한 번에 모아둔 다음 보내니 훨씬 속도가 빠르고 별다른 정규식을 검사하지 않으니 더더욱 속도는 빠를 수밖에 없다.
2. BufferedReader는 별다른 정규식을 검사하지 않는다.
   - Scanner는 정규식을 불필요할 정도(?)로 많이 검사하기 때문이다. (물론 장점도 있다. 강력한 정규식 검사로 인해 여러 예외적인 입력 값에 대해서도 입력받은 값이 특정 타입으로 변환 할 수 있는지를 정확하게 파악할 수 있다. 즉, 타입 변환의 안정성이 매우 뛰어나다는 점이다.)
