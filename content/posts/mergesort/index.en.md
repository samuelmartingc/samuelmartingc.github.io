--- 
weight: 4
title: "Mergesort"
date: "2021-05-20"
lastmod: "2021-05-20"
description: "Mergesort, an elegant algorithm to order lists"
resources:
- name: "featured-image"
  src: "mergesort.png"

tags: ["development", "algorithm"]
categories: ["Development"]

lightgallery: true
---


**Introduction**

Yesterday I was talking with a teammate about algorithms, and how some people had troubles with them, so I will create this new post to explain how mergesort works. An algorithm to order arrays/lists in O(n*log n)

**Big O Complexity**

In case you have not heard about big O notation. I will do a brief resume about it.
We usually talk about two types of complexity, space and time. And by default if it is not specified, usually everyone is talking about time.
If you try to order with a brute force algorithm an array, you will probably end with something similar to the  [bubble algorithm](https://en.wikipedia.org/wiki/Bubble_sort)

The problem with that is that the complexity is O(n^2), also called quadratic. This means that when the size of the array grows linearly the time to order it grows quadratically, and thus, this solution can't be applied to huge arrays, independently on your hardware. At some point it won't finish in less than hours or days or more.

There are better algorithms like mergesort or quicksort that have improved on this. Mergesort has a time complexity of O(n * log n), which means that if the size of your array grows linearly, the time required will grow a bit more than linearly, but it will scale pretty well.

Additional info about big O notation can be found here: [wikipedia.org/wiki/Big_O_notation](https://en.wikipedia.org/wiki/Big_O_notation)

**How does it work on a high level**

Merge sort use the pattern divide and conquer, and will reduce the problem in smaller subproblems, then solve them, and finally it will merge/join these subproblems until reach the final solution.

The steps on a high level are

1- Take the array, split it in two halves.

2- call recursively to the algorithm with each of the halves

3- if the input array has size 2 or less, it means you have the base case, order it and return the solution

4- merge the result of your current two halves


**Let's code it**

First I like to have some basic unit tests to ensure my algorithm will work, so I will write them first:
```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class MergeSortTest {

    @Test
    public void mergeSort(){
        int[] unordered = new int[]{9,7,5,2,1,3,4,6,8};
        int[] expected = new int[]{1,2,3,4,5,6,7,8,9};
        MergeSort mergeSort = new MergeSort();

        int[] ordered = mergeSort.sort(unordered);
        for (int i =0;i<ordered.length;i++){
            assertEquals(expected[i],ordered[i],"the numbers should be equal for pos=" + 1);
        }
    }

    @Test
    public void mergeSortOneElement(){
        int[] unordered = new int[]{9};
        int[] expected = new int[]{9};
        MergeSort mergeSort = new MergeSort();

        int[] ordered = mergeSort.sort(unordered);
        for (int i =0;i<ordered.length;i++){
            assertEquals(expected[i],ordered[i],"the numbers should be equal for pos=" + 1);
        }
    }

    @Test
    public void mergeSortEmpty(){
        int[] unordered = new int[]{};
        int[] expected = new int[]{};

        MergeSort mergeSort = new MergeSort();

        int[] ordered = mergeSort.sort(unordered);
        for (int i =0;i<ordered.length;i++){
            assertEquals(expected[i],ordered[i],"the numbers should be equal for pos=" + 1);
        }
    }
}

```

Now that we can test safely our algorithm, let's implement it according to the high level steps I wrote above.


```java
public class MergeSort {

    public int[] sort(int[] array){
        int[] result = array.clone();
        innerSort(result,0,result.length -1);
        return result;
    }

    private void innerSort(int[] array, int start, int end){
        if (array.length < 2){
            return; // base case, 1 element in the array
        }
        int half = array.length / 2;
        // sort first half
        int[] left = new int[half];
        for (int i = 0;i<left.length;i++){
            left[i] = array[i];
        }
        innerSort(left,0,left.length-1);
        //sort second half
        int[] right = new int[array.length - half];
        for (int i = 0;i<right.length;i++){
            right[i] = array[half + i];
        }
        innerSort(right, 0, right.length-1);
        //merge both sub arrays
        merge(array,left,right);
    }

    private void merge(int[] array, int[] left, int[] right) {
        int posLeft = 0;
        int posRight = 0;
        for (int i=0;i<array.length;i++){
            if ( (right.length <= posRight) || (!(left.length <= posLeft) && (left[posLeft] < right[posRight])) ){
                array[i] = left[posLeft++];
            } else {
                array[i] = right[posRight++];
            }
        }
    }
}

```

In this implementation I have copied the original array to not modify it. As you see the main steps are there. Writing the base case, calling recursively the algorithm with both halves ,and after you receive both halves ordered, call merge function to group them.


**Other examples**

This is one of many implementations for this algorithm, if you want to see others, you can check these two websites as well.

[Baeldung](https://www.baeldung.com/java-merge-sort)

[geeksforgeeks](https://www.geeksforgeeks.org/merge-sort/)


