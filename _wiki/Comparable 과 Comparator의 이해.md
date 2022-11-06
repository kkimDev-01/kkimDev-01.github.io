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

### 요약

1. 자기 자신과 매개변수를 비교한다.

2. compareTo 메소드를 반드시 구현해야한다.

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

### 테스트 코드

```java
public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반


		int isBig = a.compareTo(b);	// a자기자신과 b객체를 비교한다.

		if(isBig > 0) {
			System.out.println("a객체가 b객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("a객체가 b객체보다 작습니다.");
		}

	}

}

class Student implements Comparable<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compareTo(Student o) {
		return this.age - o.age;
	}
}
```

### 주의할 점 : 자료형의 범위 (Overflow, Underflow)

- 차이(-)를 리턴값으로 사용하는 간소화 버전의 경우, 자료형의 범위에 걸친, 즉 경계값에 해당되는 숫자가 비교대상에 포함된 경우 overflow, underflow가 발생하여 비교결과가 달라진다.
- 따라서 안전하게 사용하려면, 간소화 버전을 사용하지 말아야 한다.
  - if문과 >, <, == 같은 비교연산자를 통해 비교하여 음수, 0, 양수를 반환하도록 하는 것이 안전하다.

```java
public class Test {
	public static void main(String[] args) {

		int min = Integer.MIN_VALUE;	// MIN_VALUE는 -2,147,483,648 이다.
		int max = Integer.MAX_VALUE;	// MAX_VALUE는 2,147,483,647 이다.

		System.out.println("min - 1 = " + (min - 1)); //2,147,483,647
		System.out.println("max + 1 = " + (max + 1)); //-2,147,483,648
	}
}
```

- 예로들어 다음과 같은 두 값이 있다고 해보자.
- o1 = 1, o2 = -2,147,483,648
- 두 수를 위 처럼 return o1 - o2; 형식으로 하면 어떻게 될까?
  - 1 - (-2,147,483,648) = 2,147,483,648 이 되어야 하지만
  - -2,147,483,648 이 되어 음수값이 나와버린다.
  - 그러면 1인 o1이 -2,147,483,648인 o2보다 작다는 상황이 와버린다.
- compareTo를 구현하거나(Compare), 이후 설명 할 compare을 구현 할 때(Comparator) 대소비교에 있어 이러한 Overflow 혹은, Underflow가 발생할 여지가 있는지를 반드시 확인하고 사용해야 한다.

## Comparator

### compare(T o1, T o2)

- API 문서 : docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary
- compare 메소드를 재정의(Override, 구현) 해줘야 한다

### 자기 자신의 상태가 어떻던 상관없이 파라미터로 들어오는 두 객체를 비교

### util 패키지에 있으므로 import 해줘야 한다.

### 예시1

```java
import java.util.Comparator;	// import 필요
class Student implements Comparator<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student o1, Student o2) {

		// o1의 학급이 o2의 학급보다 크다면 양수
		if(o1.classNumber > o2.classNumber) {
			return 1;
		}
		// o1의 학급이 o2의 학급과 같다면 0
		else if(o1.classNumber == o2.classNumber) {
			return 0;
		}
		// o1의 학급이 o2의 학급보다 작다면 음수
		else {
			return -1;
		}
	}
}
```

### 예시2 : 간소화

```java
import java.util.Comparator;	// import 필요
class Student implements Comparator<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student o1, Student o2) {

		/*
		 * 만약 o1의 classNumber가 o2의 classNumber보다 크다면 양수가 반환 될 것이고,
		 * 같다면 0을, 작다면 음수를 반환할 것이다.
		 */
		return o1.classNumber - o2.classNumber;
	}
}
```

### 테스트 코드

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반

		// a객체와는 상관 없이 b와 c객체를 비교한다.
		int isBig = a.compare(b, c);

		if(isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

	}
}

class Student implements Comparator<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student o1, Student o2) {
		return o1.classNumber - o2.classNumber;
	}
}
```

### Underflow 예시

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);					// 17살 2반
		Student b = new Student(18, Integer.MIN_VALUE);	// 18살 -2,147,483,648반
		Student c = new Student(15, 3);					// 15살 3반

		// a객체와는 상관 없이 b와 c객체를 비교한다.
		int isBig = a.compare(b, c);

		if(isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}


        //언더플로우가 발생하여 b객체가 더 큰 것으로 판단해버린다.

	}
}

class Student implements Comparator<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student o1, Student o2) {
		return o1.classNumber - o2.classNumber;
	}
}
```

### Overflow 예시

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);					// 17살 2반
		Student b = new Student(18, Integer.MAX_VALUE);	// 18살 2,147,483,647반
		Student c = new Student(15, -3);					// 15살 -3반

		// a객체와는 상관 없이 b와 c객체를 비교한다.
		int isBig = a.compare(b, c);

		if(isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

        //b객체가 c객체보다 커야함에도 작다고 나와버린다.

	}
}

class Student implements Comparator<Student> {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student o1, Student o2) {
		return o1.classNumber - o2.classNumber;
	}
}
```

### Comparator의 활용 : 익명 객체(클래스) 사용

- Comparator를 통해 compare 메소드를 사용려면 결국에는 compare메소드를 활용하기 위한 객체가 필요하게 된다.
- 어떤 객체를 통해 호출하던 상관이 없다 : 일관성이 떨어진다.
- Comparator 기능만 따로 두고싶다면 어떻게 해야할까? : 익명 객체(클래스)를 활용한다.

### 익명 객체

- 이름이 정의되지 않은 객체
- 클래스 이름으로 정의되지 않는 객체
- 특정 구현 부분만 따로 사용한다거나, 부분적으로 기능을 일시적으로 바꿔야 할 경우
- 이름이 정의되지 않기 때문에 특정 타입이 존재하는 것이 아니기 때문에, 반드시 익명 객체의 경우는 상속할 대상이 있어야 한다는 것이다.
- 상속이라 함은 class의 extends 뿐만 아니라 interface의 implements 또한 마찬가지다.

- 일반 케이스

```java
public class Anonymous {
	public static void main(String[] args) {

		Rectangle a = new Rectangle();
		ChildRectangle child = new ChildRectangle();

		System.out.println(a.get());		// 20
		System.out.println(child.get());	// 10 * 20 * 40
	}
}

class ChildRectangle extends Rectangle {

	int depth = 40;

	@Override
	int get() {
		return width * height * depth;
	}
}

class Rectangle {

	int width = 10;
	int height = 20;

	int get() {
		return height;
	}
}
```

- 익명 객체 사용 케이스 : extends

```java
public class Anonymous {
	public static void main(String[] args) {

		Rectangle a = new Rectangle();

		Rectangle anonymous = new Rectangle() {
			int depth = 40;
			@Override
			int get() {
				return width * height * depth;
			}
		};

		System.out.println(a.get());			// 20
		System.out.println(anonymous.get());	// 10 * 20 * 40
	}
}
class Rectangle {

	int width = 10;
	int height = 20;

	int get() {
		return height;
	}
}
```

- 익명 객체 사용 케이스 : implements

```java
public class Anonymous {
	public static void main(String[] args) {

		Rectangle a = new Rectangle();

		Shape anonymous = new Shape() {
			int depth = 40;

			@Override
			public int get() {
				return width * height * depth;
			}
		};

		System.out.println(a.get());			// Shape 인터페이스를 구현한 Rectangle
		System.out.println(anonymous.get());	// Shape 인터페이스를 구현한 익명 객체
	}

}

class Rectangle implements Shape {
	int depth = 40;

	@Override
	public int get() {
		return width * height * depth;
	}
}

interface Shape {

	int width = 10;
	int height = 20;

	int get();
}

```

### Comparator 익명 객체 사용례

- 익명 객체의 경우, 필요에 따라 main함수 밖에 정적(static) 타입으로 선언해도 되고, main안에 지역변수처럼 non-static으로 생성해도 된다.
- 가독성 측면에서 두 번째 방식이 좀 더 잘 보이기 때문에 두 번째 생성 방식(static 방식)으로 주로 쓴다.

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {

		// 익명 객체 구현방법 1
		Comparator<Student> comp1 = new Comparator<Student>() {
			@Override
			public int compare(Student o1, Student o2) {
				return o1.classNumber - o2.classNumber;
			}
		};
	}

	// 익명 객체 구현 2
	public static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
}


// 외부에서 익명 객체로 Comparator가 생성되기 때문에 클래스에서 Comparator을 구현 할 필요가 없어진다.
class Student {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

}
```

- 테스트 코드

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반

		// comp 익명객체를 통해 b와 c객체를 비교한다.
		int isBig = comp.compare(b, c);

		if(isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

	}

	public static Comparator<Student> comp = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
}

class Student {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

}
```

### Comparator는 익명객체로 여러개를 생성할 수 있지만, Comparable의 경우 compareTo 하나 밖에 구현할 수 없다

- 익명 객체를 사용하면 좋은 점이 하나 더 있다
  - 익명 객체를 가리키는 변수명만 달리하면 몇 개든 자유롭게 생성할 수 있다.
  - 즉, 익명객체를 통해 여러가지 비교 기준을 정의할 수 있다
- 보통은 Comparable은 여러분이 비교하고자 하는 가장 기본적인 설정(보통은 오름차순)으로 구현하는 경우가 많고, Comparator는 여러개를 생성할 수 있다보니 특별한 정렬을 원할 때 많이 쓰인다.
- 즉, Comparable은 기본(default) 순서를 정의하는데 사용되며, Comparator은 특별한(specific) 기준의 순서를 정의할 때 사용된다는 것이다.

```java
import java.util.Comparator;

public class Test {
	public static void main(String[] args)  {

		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반

		// 학급 기준 익명객체를 통해 b와 c객체를 비교한다.
		int classBig = comp.compare(b, c);

		if(classBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(classBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

		// 나이 기준 익명객체를 통해 b와 c객체를 비교한다.
		int ageBig = comp2.compare(b, c);

		if(ageBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(ageBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

	}

	// 학급 대소 비교 익명 객체
	public static Comparator<Student> comp = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};

	// 나이 대소 비교 익명 객체
	public static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.age - o2.age;
		}
	};
}

class Student {

	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

}
```

## 정렬과의 관계

- 디폴트 : 오름차순
  - Arrays.sort(), Collections.sort() 모두 오름차순을 기준으로 정렬이 된다
- return 선행원소 - 후행원소
  - 음수일 경우 : 두 원소의 위치를 교환 안함
  - 양수일 경우 : 두 원소의 위치를 교환 함

### 기억하기 쉬운 방법

- 선행 원소는 값이 작다고 가정하고, 후행 원소는 값이 크다고 가정

```java
/*
 [오름차순]
 작은 원소가 큰 원소보다 앞에 있으므로 오름차순이다.
 */
public int compareTo(MyClass o) {
	return this.value - o.value;
}
public int compare(Myclass o1, MyClass o2) {
	return o1.value - o2.value;
}


/*
 [내림차순]
 큰 원소가 작은 원소보다 앞에 있으므로 내림차순이다.
 */
public int compareTo(MyClass o) {
	return o.value - this.value;
}
public int compare(Myclass o1, MyClass o2) {
	return o2.value - o1.value
```

## Arrays.sort

### Comparable을 사용 : compareTo를 Override해서 사용

```java
import java.util.Arrays;

public class Test {

	public static void main(String[] args) {

		MyInteger[] arr = new MyInteger[10];

		// 객체 배열 초기화 (랜덤 값으로)
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}

		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();

		Arrays.sort(arr);

		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}

}

class MyInteger implements Comparable<MyInteger> {
	int value;

	public MyInteger(int value) {
		this.value = value;
	}

	@Override
	public int compareTo(MyInteger o) {
		return this.value - o.value;
	}

}
```

### Comparator를 사용 : Arrays.sort()에 파라미터로 익명 객체를 넘겨주어 사용

```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {

	public static void main(String[] args) {

		MyInteger[] arr = new MyInteger[10];

		// 객체 배열 초기화 (랜덤 값으로)
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}

		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();

		Arrays.sort(arr, comp);		// MyInteger에 대한 Comparator을 구현한 익명객체를 넘겨줌

		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}


	static Comparator<MyInteger> comp = new Comparator<MyInteger>() {

		@Override
		public int compare(MyInteger o1, MyInteger o2) {
			return o1.value - o2.value;
		}
	};
}
```

### 응용 : 역순정렬

- 반환값의 부호를 반대로 해주면 된다.

```java
// Comparable
public int compareTo(MyClass o) {
	return -(this.value - o.value);
}

// Comparator
public int compare(Myclass o1, MyClass o2) {
	return -(o1.value - o2.value);
}
```
