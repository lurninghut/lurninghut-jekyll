---
layout: post
title:  "Sorting algorithms - Quick View"
categories: [ Java ]
image: assets/images/sorting-algorithms-quick-view/header-1.jpg
---
Sorting is a very important concept when it comes to problem solving during any interview. 
There are a number of ways in which we can sort a given array but to find the most efficient way needs logical thinking and some imagination to write code around it.
Sorting has a cost in terms of time and space requirements and any efficient algorithm will try to optimise the time it takes to sort by reducing number of comparisons and any extra space needed. 
The most common out-of-the-box algorithms provided as part of standard libraries use Quicksort which as an average runtime of O(n log n) where n is the number of elements to be considered for sorting.

| Sorting|Average|Worst|Use case|
| ---------------|----|--------|----------|
| <img width=200/>Insertion Sort|<img width=200/> N^2 |N^2|When the size of the array is small.|
| Quick Sort | N log N | N^2 (When input array is already sorted and we choose left most element as Pivot or When input array is reverse sorted and we choose rightmost element as pivot) | Standard algorithm provided by libraries.**Pros** Can be in place without needing extra space.**Cons** Its not stable which means order of elements is not preserved when they are same.|
| Merge Sort | N log N | N log N | **Pros** Stable algorithm **Cons** Not in-place. Needs extra space.|
|Heap Sort |N log N |N log N| **Pros** In place and worst case is better than Quick Sort. |
|Radix Sort|N * k|N * k|Its a non comparison sort running in linear time. Pros Stable sort Cons Not in-place|