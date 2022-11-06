---
layout: wiki
title: Collections.binarySearch()에 대해
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

- [Collections.binarySearch() in Java with Examples
  ](https://www.geeksforgeeks.org/collections-binarysearch-java-examples/)

## API 문서의 내용

```java
// Returns index of key in sorted list sorted in
// ascending order
// The list must be sorted into ascending order according to the natural ordering of its elements (as by the sort(List) method) prior to making this call. If it is not sorted, the results are undefined. If the list contains multiple elements equal to the specified object, there is no guarantee which one will be found.
public static int binarySearch(List slist, T key)

// Returns index of key in sorted list sorted in
// order defined by Comparator c.
public static int binarySearch(List slist, T key, Comparator c)

If key is not present, then it returns "(-(insertion point) - 1)".
The insertion point is defined as the point at which the key
would be inserted into the list.
```

- 이진 탐색 : O (log2 n)
- 파라미터 2개를 받는다 : 정렬된 리스트, 찾고자 하는 값
- 파라미터 3개를 받을 수도 있다 : 정렬에 사용된 Comparator
- 사전에 반드시 정렬이 되어야 하며, 정렬된 리스트를 파라미터로 전달해야 함
- 리스트에 없는 값, 즉 값을 찾지 못하면, 그 값이 정렬 순서 상으로 들어가야 되는 위치(인덱스)를 구하고, -(index) -1 값을 리턴함
- 중복된 값이 검색 대상이라면, 어떤 값이 탐색의 결과가 될 지 알 수 없음

## Arrays.binarysearch() 와 비교

- Arrays.binarysearch()는 primitive Type의 탐색 쓰이지만, Collections.sort()는 Object형의 탐색에도 쓸 수 있다.

## 사용례

### 파라미터 2개 : 기본

```java

// Java program to demonstrate working of Collections.
// binarySearch()
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class GFG {
    public static void main(String[] args)
    {
        List<Integer> al = new ArrayList<Integer>();
        al.add(1);
        al.add(2);
        al.add(3);
        al.add(10);
        al.add(20);

        // 10 is present at index 3.
        int index = Collections.binarySearch(al, 10);
        System.out.println(index);

        // 13 is not present. 13 would have been inserted
        // at position 4. So the function returns (-4-1)
        // which is -5.
        index = Collections.binarySearch(al, 13);
        System.out.println(index);
    }
}

```

### 파라미터 3개 : reverse() 사용

```java


// Java program to demonstrate working of Collections.
// binarySearch() in an array sorted in descending order.
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class GFG {
    public static void main(String[] args)
    {
        List<Integer> al = new ArrayList<Integer>();
        al.add(100);
        al.add(50);
        al.add(30);
        al.add(10);
        al.add(2);

        // The last parameter specifies the comparator
        // method used for sorting.
        int index = Collections.binarySearch(
            al, 50, Collections.reverseOrder());

        System.out.println("Found at index " + index);
    }
}
```

### 파라미터 3개 : 사용자 정의 Comparator 사용

```java
// Java program to demonstrate working of Collections.
// binarySearch() in a list of user defined objects
import java.util.*;

class Binarysearch {
    public static void main(String[] args)
    {
        // Create a list
        List<Domain> l = new ArrayList<Domain>();
        l.add(new Domain(10, "quiz.geeksforgeeks.org"));
        l.add(new Domain(20, "practice.geeksforgeeks.org"));
        l.add(new Domain(30, "code.geeksforgeeks.org"));
        l.add(new Domain(40, "www.geeksforgeeks.org"));

        Comparator<Domain> c = new Comparator<Domain>() {
            public int compare(Domain u1, Domain u2)
            {
                return u1.getId().compareTo(u2.getId());
            }
        };

        // Searching a domain with key value 10. To search
        // we create an object of domain with key 10.
        int index = Collections.binarySearch(
            l, new Domain(10, null), c);
        System.out.println("Found at index  " + index);

        // Searching an item with key 5
        index = Collections.binarySearch(
            l, new Domain(5, null), c);
        System.out.println(index);
    }
}

// A user-defined class to store domains with id and url
class Domain {
    private int id;
    private String url;

    // Constructor
    public Domain(int id, String url)
    {
        this.id = id;
        this.url = url;
    }

    public Integer getId() { return Integer.valueOf(id); }
}

```
