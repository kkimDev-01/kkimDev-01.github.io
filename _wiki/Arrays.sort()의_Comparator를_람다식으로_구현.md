---
layout: wiki
title: Arrays.sort()의_Comparator를_람다식으로_구현
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

- [[백준] 11650번 : 좌표 정렬하기 - JAVA [자바]](https://st-lab.tistory.com/110)

## 람다식

```java
(매개변수1,...) -> {함수;}
```

```java
// 람다식 X
public int sum(int a, int b) {
	return a + b;
}

// 람다식 O
(int a, int b) -> {return a + b;}
```

```java
// 람다식 X
int c = sum(a, b);
public int sum(int a, int b) {
	return a + b;
}

// 람다식 O
int c = (int a, int b) -> {return a + b;}
```

- 장점
  - 람다식을 쓰지 않을 때보다 훨씬 코드가 간결해진다.
  - 일회성으로 함수를 써야할 경우, 굳이 메소드 하나를 작성하기보다는 람다식으로 구현하는 것이 개발자의 의도라던가 가독성 등에서 훨씬 좋아지는 장점이 있다.

## Arrays.sort()와 람다식

- 자바 API

  - public static <T> void sort(T[] a,
    Comparator<? super T> c)

  1. 첫번째 인자 : 제너릭타입 객체배열( T[] )

  - 클래스, 오브젝트, 자료형 등 어떠한 타입이든 상관없이 배열의 형태로 받을 수 있다는 의미

  2. 두번째 인자 : Comparator<? super T>

  - <? super T>
    - 상속관계에 있는 타입만 자료형으로 받겠다는 의미
    - T 타입이 자식클래스가 되고, T의 상속관계에 있는 타입까지만 허용하겠다는 의미
  - Comparator 의 경우 람다식으로 표현 할 수 있다.
    - compare 메소드를 오버라이딩하여 compare 메소드 안에 자신만의 비교 방법(우선순위 판단)을 구현
    - Comparator에 대해 익명 클래스를 생성할 때 compare을 T[] 타입을 가진 매개변수를 람다식으로 바꾸어 쓸 수 있다.
  - compare 메소드 리턴 타입은 int형이다.
    - compare 메소드는 3가지 리턴 값에 의해 위치를 바꿀지 결정하게 된다.
      - 양의 정수
      - 0
      - 음의 정수
    - 양수일경우 Arrays.sort()에서 정렬 알고리즘에 의해 위치를 바꾸고, 0 이나 음의 정수인 경우는 두 객체의 위치는 바뀌지 않는다.
    - 예로들어 { 2, 1, 3 } 이라는 배열이 있고, public int compare(int a1, int a2) { return a1 - a2 } 가 있다고 가정해보자.
    - 그렇다면 맨 처음 a1 은 2 가 될테고, a2 는 1이 된다. 즉, 2 - 1 = 1 이므로 양수가 반환되기 때문에 a1 과 a2, 즉 2 와 1 의 위치가 서로 바뀌게 된다. 그러면 { 1, 2, 3 } 이 되겠다.
    - 그 다음 a1, a2 는 각각 2 와 3이 될테고, 2 - 3 = -1 이므로 음수가 반환되어 두 객체 2와 3은 위치가 바뀌지 않는다.

```java
//람다식X
Arrays.sort(arr, new Comparator<int[]>() {
	@Override
	public int compare(int[] e1, int[] e2) {
		if(e1[0] == e2[0]) {		// 첫번째 원소가 같다면 두 번째 원소끼리 비교
			return e1[1] - e2[1];
		}
		else {
			return e1[0] - e2[0];
		}
	}
});

//람다식O
Arrays.sort(arr, (e1, e2) -> {
	if(e1[0] == e2[0]) {
		return e1[1] - e2[1];
	}
	else {
		return e1[0] - e2[0];
	}
});
```
