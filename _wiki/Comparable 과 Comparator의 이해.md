---
layout: wiki
title: Comparable 과 Comparator의 이해
summary:
date: 2022-11-06 00:00:00 +0900
updated: 2022-11-06 00:00:00 +0900
tag:
toc: true
public: true
parent: [[]]
latex: true
---

## 참고문헌

- [자바 [JAVA] - Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)

## 공통

- 객체를 비교할 수 있게 만든다
  - primitive 타입의 실수 변수(byte, int, double 등등..)의 경우 부등호를 갖고 쉽게 두 변수를 비교할 수 있다.
  - 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선순위를 갖는지 판단 할 수가 없다.
  - 이러한 문제점을 해결하기 위해, 즉 '객체'를 비교할 수 있도록 Comparable 또는 Comparator가 쓰인다.

## 예시 객체 : Student

```java
class Student {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
}
```

## Comparable

### compareTo(T o)

- API 문서 : docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#method.summary
- compareTo 메소드를 재정의(Override, 구현) 해줘야 한다

### 자기 자신과 파라미터로 들어오는 객체를 비교

### lang 패키지에 있기 때문에 import 를 해줄 필요가 없다.

### 예시1

```java
class Student implements Comparable<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compareTo(Student o) {

		// 자기자신의 age가 o의 age보다 크다면 양수
		if(this.age > o.age) {
			return 1;
		}
		// 자기 자신의 age와 o의 age가 같다면 0
		else if(this.age == o.age) {
			return 0;
		}
		// 자기 자신의 age가 o의 age보다 작다면 음수
		else {
			return -1;
		}
	}
}
```

### 예시2 : -1, 0, 1로 반환할 수도 있으나, 그냥 두 비교대상의 값 차이를 반환해도 된다.

```java
class Student implements Comparable<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compareTo(Student o) {

		/*
		 * 만약 자신의 age가 o의 age보다 크다면 양수가 반환 될 것이고,
		 * 같다면 0을, 작다면 음수를 반환할 것이다.
		 */
		return this.age - o.age;
	}
}
```

## Comparator

### compare(T o1, T o2)

- API 문서 : docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary
- compare 메소드를 재정의(Override, 구현) 해줘야 한다

### 자기 자신의 상태가 어떻던 상관없이 파라미터로 들어오는 두 객체를 비교

### util 패키지에 있으므로 import 해줘야 한다.
