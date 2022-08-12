## Sorting with while loops
While loops help with running code block repeatedly over a condition
until failure of the condition. Sorting algorithms need to perform repeated
comparisons and swapping operations over certain conditions and sometimes
it becomes difficult to understand the loop start and end indexes. While loops 
help in exactly pointing out what indexes mean and how they are changed within the 
algorithm. Following are 3 common sorting algorithms which will use while loops
to perform sorting:
#### Bubble Sort
Bubble sort's core logic it do N-1 loops over the set of N items and on each pass move the highest element to the
end of the array by comparing and swapping the neighbouring item.

Example of 1st Pass :

int[] data = {4,3,2,1}

| i0 | i1 | i2 | i3 |
|---|---|---|---|
| _**4**_ | 3 | 2 | 1 |
| 3| _**4**_ | 2 | 1 |
| 3| 2 | _**4**_ | 1 |
| 3| 2 | 1 | _**4**_ |


~~~java
private static void bubble(int[] data) {
        int outerStart = 0;
        int outerEnd = data.length - 1;
        int innerStart;
        int innerEnd;

        while (outerStart <= outerEnd) {
            innerStart = 1;
            innerEnd = outerEnd - outerStart;
            while (innerStart <= innerEnd) {
                if (data[innerStart] < data[innerStart - 1]) {
                    int temp = data[innerStart];
                    data[innerStart] = data[innerStart - 1];
                    data[innerStart - 1] = temp;
                }
                innerStart++;
            }
            outerStart++;
        }
    }
~~~
#### Insertion Sort
Insertion sort picks an element (key) and tries to find the correct index on its left subarray where the key belongs and then does the swapping. This results in less number of swappings than bubble sort.

~~~java
private static void insertion(int[] data) {
        int outerStart = 1;
        int outerEnd = data.length - 1;
        while (outerStart <= outerEnd) {
            int innerStart = outerStart;
            int key = data[innerStart];
            while (innerStart > 0 && key < data[innerStart - 1]) {
                data[innerStart] = data[innerStart - 1];
                innerStart--;
            }
            data[innerStart] = key;
            outerStart++;
        }
    }
~~~
#### Selection Sort
Selection sort is the simplest to code, where, for every pass in the loop, index of the minimum element
in the pass is found and then the item is swapped with the current loop index item. 
~~~java
private static void selection(int[] data) {
        int outerStart = 0;
        int outerEnd = data.length - 2;
        while (outerStart <= outerEnd) {
            int innerStart = outerStart;
            int innerEnd = data.length - 1;
            int smallIndex = innerStart;
            while (innerStart <= innerEnd) {
                if (data[innerStart] < data[smallIndex]) {
                    smallIndex = innerStart;
                }
                innerStart++;
            }
            int temp = data[outerStart];
            data[outerStart] = data[smallIndex];
            data[smallIndex] = temp;
            outerStart++;
        }
    }
~~~