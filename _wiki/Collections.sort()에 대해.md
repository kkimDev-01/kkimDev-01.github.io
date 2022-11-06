---
layout: wiki
title: Collections.sort()에 대해
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

- [Collections.sort() in Java with Examples](https://www.geeksforgeeks.org/collections-sort-java-examples/)

## 개요 : java.util.Collections.sort()

- Timsort를 사용한다 : [[Tim Sort]]
- 평균 O(n log2(n))이고, 최악의 경우에도 O(n log2(n))이다.
- Arrays.sort()와 비교
  - Arrays.sort()는 dual pivot quick sort이며, 최악의 경우 평균 O(n log2(n))이고, 최악의 경우에는 O(n^2)이다.
  - Arrays.sort()는 array에만 쓸 수 있다.
  - Collections.sort는 Array, List, linkedList, queue 등에도 쓸 수 있다.
  - Arrays.sort()는 primitive Type의 정렬 쓰이지만, Collections.sort()는 Object형의 정렬에도 쓸 수 있다 (comparator 구현해서) : [[Comparable 과 Comparator의 이해]]

## 사용법

1. 기본 (오름차순 정렬)

```java
import java.util.*;

public class Collectionsorting
{
    public static void main(String[] args)
    {
        // Create a list of strings
        ArrayList<String> al = new ArrayList<String>();
        al.add("Geeks For Geeks");
        al.add("Friends");
        al.add("Dear");
        al.add("Is");
        al.add("Superb");

        /* Collections.sort method is sorting the
        elements of ArrayList in ascending order. */
        Collections.sort(al);

        // Let us print the sorted list
        System.out.println("List after the use of" +
                           " Collection.sort() :\n" + al);
    }
}
```

```java
List after the use of Collection.sort() :
[Dear, Friends, Geeks For Geeks, Is, Superb]
```

2. 내림차순 정렬

```java

// Java program to demonstrate working of Collections.sort()
// to descending order.
import java.util.*;

public class Collectionsorting
{
    public static void main(String[] args)
    {
        // Create a list of strings
        ArrayList<String> al = new ArrayList<String>();
        al.add("Geeks For Geeks");
        al.add("Friends");
        al.add("Dear");
        al.add("Is");
        al.add("Superb");

        /* Collections.sort method is sorting the
        elements of ArrayList in ascending order. */
        Collections.sort(al, Collections.reverseOrder());

        // Let us print the sorted list
        System.out.println("List after the use of" +
                           " Collection.sort() :\n" + al);
    }
}
```

```java
List after the use of Collection.sort() :
[Superb, Is, Geeks For Geeks, Friends, Dear]
```

3. Object 정렬 : Comparator 인터페이스 구현

comparator 인터페이스 : [[Comparable 과 Comparator의 이해]]

```java

// Java program to demonstrate working of Comparator
// interface and Collections.sort() to sort according
// to user defined criteria.
import java.util.*;
import java.lang.*;
import java.io.*;

// A class to represent a student.
class Student
{
    int rollno;
    String name, address;

    // Constructor
    public Student(int rollno, String name,
                               String address)
    {
        this.rollno = rollno;
        this.name = name;
        this.address = address;
    }

    // Used to print student details in main()
    public String toString()
    {
        return this.rollno + " " + this.name +
                           " " + this.address;
    }
}

class Sortbyroll implements Comparator<Student>
{
    // Used for sorting in ascending order of
    // roll number
    public int compare(Student a, Student b)
    {
        return a.rollno - b.rollno;
    }
}

// Driver class
class Main
{
    public static void main (String[] args)
    {
        ArrayList<Student> ar = new ArrayList<Student>();
        ar.add(new Student(111, "bbbb", "london"));
        ar.add(new Student(131, "aaaa", "nyc"));
        ar.add(new Student(121, "cccc", "jaipur"));

        System.out.println("Unsorted");
        for (int i=0; i<ar.size(); i++)
            System.out.println(ar.get(i));

        Collections.sort(ar, new Sortbyroll());

        System.out.println("\nSorted by rollno");
        for (int i=0; i<ar.size(); i++)
            System.out.println(ar.get(i));
    }
}
```

```java
Unsorted
111 bbbb london
131 aaaa nyc
121 cccc jaipur

Sorted by rollno
111 bbbb london
121 cccc jaipur
131 aaaa nyc
```

4. Arrays를 Collections.sort()로 정렬 : List형으로 변경 필요

```java

// Using Collections.sort() to sort an array
import java.util.*;
public class Collectionsort
{
    public static void main(String[] args)
    {
        // create an array of string objs
        String domains[] = {"Practice", "Geeks",
                             "Code", "Quiz"};

        // Here we are making a list named as Collist
        List colList =
            new ArrayList(Arrays.asList(domains));

        // Collection.sort() method is used here
        // to sort the list elements.
        Collections.sort(colList);

        // Let us print the sorted list
        System.out.println("List after the use of" +
                           " Collection.sort()  :\n" +
                           colList);
    }
}
```

```java
List after the use of Collection.sort()  :
[Code, Geeks, Practice, Quiz]
```
