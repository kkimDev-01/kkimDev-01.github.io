---
layout: wiki
title: Dual-Pivot-Quick-Sort
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

- [[Dual-Pivot Quick Sort] 두 개의 피봇으로 퀵 정렬
  ](https://cs-vegemeal.tistory.com/53)

## 개요

- java.util.Array.sort가 사용 하는 알고리즘이다.
- insertion-sort와 quick-sort를 합쳤다.
  - 이 말의 의미는 소스에서 알 수 있다.
  - java.util.DualPivotQuicksort
- insertion-sort
  - 데이터 양을 먼저 예외처리마냥 판단해서, 적은양의 데이터에 대해서는 삽입정렬로 풀어내고 있다.
  - pivot을 5개를 구하는데 (데이터를 7등분), pivot 간의 정렬에서도 삽입정렬을 사용한다.
  - 데이터의 7분위, 즉 1/7은 비트연산으로 어림계산된다.
- dual-pivot-quick-sort
  - 본격적인 정렬은 quick-sort로 구현된다.
  - 다만 피벗을 두 개 사용한다(2/7분위수 = 약 30%, 4/7분위수 = 약 60%)
  - 첫번째와 마지막에 포인터를 두고, 포인터를 각각 우로/좌로 전진시켜가면서 두 피벗과 비교한다.
    - 왼쪽 포인터가 작은 피벗보다 큰 값을 만나면 멈추고, 오른쪽 포인터가 큰 피벗보다 작은 값을 만나면 멈춘다.
    - 멈춘 두 포인터 사이의 데이터들에 대해서, 1그룹(~작.포), 2그룹(작.포~큰.포), 3그룹(큰.포~)로 나눈다.
    - 각 세 그룹에 대해서 dual-pivot-quick-sort를 반복한다(재귀).

## Source : java.util.DualPivotQuicksort

- 소스 중 quick-sort부분은 아래과 같다.

```java

        // Inexpensive approximation of length / 7
        int seventh = (length >> 3) + (length >> 6) + 1;

        /*
         * Sort five evenly spaced elements around (and including) the
         * center element in the range. These elements will be used for
         * pivot selection as described below. The choice for spacing
         * these elements was empirically determined to work well on
         * a wide variety of inputs.
         */
        int e3 = (left + right) >>> 1; // The midpoint
        int e2 = e3 - seventh;
        int e1 = e2 - seventh;
        int e4 = e3 + seventh;
        int e5 = e4 + seventh;

        // Sort these elements using insertion sort
        if (a[e2] < a[e1]) { char t = a[e2]; a[e2] = a[e1]; a[e1] = t; }

        if (a[e3] < a[e2]) { char t = a[e3]; a[e3] = a[e2]; a[e2] = t;
            if (t < a[e1]) { a[e2] = a[e1]; a[e1] = t; }
        }
        if (a[e4] < a[e3]) { char t = a[e4]; a[e4] = a[e3]; a[e3] = t;
            if (t < a[e2]) { a[e3] = a[e2]; a[e2] = t;
                if (t < a[e1]) { a[e2] = a[e1]; a[e1] = t; }
            }
        }
        if (a[e5] < a[e4]) { char t = a[e5]; a[e5] = a[e4]; a[e4] = t;
            if (t < a[e3]) { a[e4] = a[e3]; a[e3] = t;
                if (t < a[e2]) { a[e3] = a[e2]; a[e2] = t;
                    if (t < a[e1]) { a[e2] = a[e1]; a[e1] = t; }
                }
            }
        }

        // Pointers
        int less  = left;  // The index of the first element of center part
        int great = right; // The index before the first element of right part

        if (a[e1] != a[e2] && a[e2] != a[e3] && a[e3] != a[e4] && a[e4] != a[e5]) {
            /*
             * Use the second and fourth of the five sorted elements as pivots.
             * These values are inexpensive approximations of the first and
             * second terciles of the array. Note that pivot1 <= pivot2.
             */
            char pivot1 = a[e2];
            char pivot2 = a[e4];

            /*
             * The first and the last elements to be sorted are moved to the
             * locations formerly occupied by the pivots. When partitioning
             * is complete, the pivots are swapped back into their final
             * positions, and excluded from subsequent sorting.
             */
            a[e2] = a[left];
            a[e4] = a[right];

            /*
             * Skip elements, which are less or greater than pivot values.
             */
            while (a[++less] < pivot1);
            while (a[--great] > pivot2);

            /*
             * Partitioning:
             *
             *   left part           center part                   right part
             * +--------------------------------------------------------------+
             * |  < pivot1  |  pivot1 <= && <= pivot2  |    ?    |  > pivot2  |
             * +--------------------------------------------------------------+
             *               ^                          ^       ^
             *               |                          |       |
             *              less                        k     great
             *
             * Invariants:
             *
             *              all in (left, less)   < pivot1
             *    pivot1 <= all in [less, k)     <= pivot2
             *              all in (great, right) > pivot2
             *
             * Pointer k is the first index of ?-part.
             */
            outer:
            for (int k = less - 1; ++k <= great; ) {
                char ak = a[k];
                if (ak < pivot1) { // Move a[k] to left part
                    a[k] = a[less];
                    /*
                     * Here and below we use "a[i] = b; i++;" instead
                     * of "a[i++] = b;" due to performance issue.
                     */
                    a[less] = ak;
                    ++less;
                } else if (ak > pivot2) { // Move a[k] to right part
                    while (a[great] > pivot2) {
                        if (great-- == k) {
                            break outer;
                        }
                    }
                    if (a[great] < pivot1) { // a[great] <= pivot2
                        a[k] = a[less];
                        a[less] = a[great];
                        ++less;
                    } else { // pivot1 <= a[great] <= pivot2
                        a[k] = a[great];
                    }
                    /*
                     * Here and below we use "a[i] = b; i--;" instead
                     * of "a[i--] = b;" due to performance issue.
                     */
                    a[great] = ak;
                    --great;
                }
            }

            // Swap pivots into their final positions
            a[left]  = a[less  - 1]; a[less  - 1] = pivot1;
            a[right] = a[great + 1]; a[great + 1] = pivot2;

            // Sort left and right parts recursively, excluding known pivots
            sort(a, left, less - 2, leftmost);
            sort(a, great + 2, right, false);\
```
