---
layout: wiki
title: insertion-sort
summary:
date: 2022-10-30 00:00:00 +0900
updated: 2022-10-30 00:00:00 +0900
tag: Algorithms
toc: true
public: true
parent: [[1427번:소프트인사이드]]
latex: true
---

## 참고문헌

- [[알고리즘] 삽입 정렬(insertion sort)이란
  ](https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html)

## 삽입정렬이란

- 과정
  - (오름차순 정렬) 두번째 요소부터 잡아서 아래의 과정을 반복한다
    1.  바로앞 요소와 비교한다. 만약 지금 잡은 요소가 더 작다면 위치를 기억한다 (앞의 요소는 인덱스+1, 잡은 요소는 인덱스-1)
    2.  이런식으로 앞에 있는 요소들을 차례로 비교해가면서 더 작은 요소가 없을 때까지 반복한다.
    3.  기억해놓은 인덱스를 토대로, 요소들을 이동시킨다.
- 특징
  - 장점
    - 대부분의 요소들이 이미 정렬되어 있는 경우에 매우 효율적이다(최선의 경우).
  - 단점
    - 비교적 요소 이동이 많다.
    - 요소의 수가 많고, 요소의 크기가 클 경우 적합하지 않다.
- 시간복잡도
  - 최선의 경우 : 이미 정렬되어 있는 경우
    - 비교작업
      - 각 요소마다 바로 앞의 하나의 요소와 비교하고 끝난다
      - 총 비교횟수 : n-1
    - 교체작업
      - 교체가 일어나지 않는다.
    - 시간복잡도 : O(n-1) = O(n)
  - 최악의 경우 : 역순으로 정렬되어 있는 경우
    - 비교작업
      - 각 요소마다 그 앞의 모든 요소와 비교를 해야 하므로, i번째 요소는 i-1번 비교를 해야 한다
      - 1 + 2 + ... + n-1
      - 총 비교횟수 = n(n-1)/2
    - 교체작업
      - 각 요소마다, 앞의 요소들을 하나씩 밀고 해당 요소는 맨앞에 놓아야 하기 때문에, i번째 요소의 경우 i만큼의 교체가 이루어진다
      - 2 + 3 + ... n
      - 총 교체횟수 : n(n-1)/2 + n -1
    - 시간복잡도 : O(n^2) \* 2 = O(n^2)
- 요약
  - 삽입정렬은 정렬이 어느정도 된 데이터의 경우 효과적이다.