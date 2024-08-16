---
description: For C enthusiasts
---

# 13 - C Advanced

## Array vs "Pointer to Array"

So far we know, an array is just **a const pointer** to the **first element** of the array.

Consider the following program:

```c
#include<stdio.h>
 
int main()
{
  int arr[5] = { 10, 20, 30, 40, 50 };
  int* ptr = arr;
 
  printf("%p 0:%d 1:%d 2:%d\n", (void*)arr, *arr, *(arr+1), *(arr+2));
  printf("%p 0:%d 1:%d 2:%d\n", (void*)arr, arr[0], arr[1], arr[2]);
  printf("%p 0:%d 1:%d 2:%d\n", (void*)ptr, *ptr, *(ptr+1), *(ptr+2));
  printf("%p 0:%d 1:%d 2:%d\n", (void*)ptr, ptr[0], ptr[1], ptr[2]);
  
  return 0;
}
```

Pointer **ptr** points to the **first element** of the array.&#x20;

Pointer **arr** also points to the **first element** of the array.&#x20;

Similarly, we can declare a pointer that can **point to whole array** instead of only one element of the array. This pointer is useful when talking about multidimensional arrays. This pointer is known as **"pointer to an array"**

Syntax:

```c
data_type (*var_name)[size_of_array];

i.e.
int (*ptrToArr10)[10];
```

Here **ptrToArr10** is pointer that can point to an array of 10 integers. Since subscript has higher precedence than indirection, it is necessary to enclose the indirection operator and pointer name inside parentheses.

Here the **type of ptrToArr10** is "pointer to an array of **10** integers"  **int (\*)\[10]**, but **not int\***

&#x20;**Note :** The pointer that points to the 0th element of array and the pointer that points to the whole array are totally different. Refer to the following program:

```c
// C program to understand difference between
// pointer to an integer and pointer to an
// array of integers.
#include<stdio.h>
 
int main()
{
    // Pointer to an integer
    int *p;
     
    // Pointer to an array of 5 integers
    int (*ptr)[5];
    int arr[5];
     
    // Points to 0th element of the arr.
    p = arr;
     
    // Points to the whole array arr.
    ptr = &arr;
     
    printf("p = %p, ptr = %p\n", p, ptr);
     
    p++;
    ptr++;
     
    printf("p = %p, ptr = %p\n", p, ptr);
     
    return 0;
}

/* 
Output 
------
p = 0x7fff4f32fd50, ptr = 0x7fff4f32fd50
p = 0x7fff4f32fd54, ptr = 0x7fff4f32fd64
*/
```

**p**: is pointer to 0th element of the array arr, while **ptr** is a pointer that points to the whole array arr. \
&#x20;

* The base type of p is int while base type of ptr is ‘an array of 5 integers’.
* We know that the pointer arithmetic is performed relative to the base size, so if we write ptr++, then the pointer ptr will be **shifted forward by 20 bytes**.

The following figure shows the pointer p and ptr. Darker arrow denotes pointer to an array.&#x20;

![](.gitbook/assets/ptr\_to\_arr.png)

On dereferencing a pointer expression we get a value pointed to by that pointer expression. Pointer to an array points to an array, so on dereferencing it, we should get the array, and the name of array denotes the base address. So whenever a pointer to an array is dereferenced, we get the base address of the array to which it points.

```c
// C program to illustrate sizes of
// pointer of array
#include<stdio.h>
 
int main()
{
    int arr[] = { 3, 5, 6, 7, 9 };
    int *p = arr;
    int (*ptr)[5] = &arr;
     
    printf("p = %p, ptr = %p\n", p, ptr);
    printf("*p = %d, *ptr = %p\n", *p, *ptr);
     
    printf("sizeof(p) = %lu, sizeof(*p) = %lu\n",
                          sizeof(p), sizeof(*p));
    printf("sizeof(ptr) = %lu, sizeof(*ptr) = %lu\n",
                        sizeof(ptr), sizeof(*ptr));
    return 0;
}

/*
Output 
------
p = 0x7ffde1ee5010, ptr = 0x7ffde1ee5010
*p = 3, *ptr = 0x7ffde1ee5010
sizeof(p) = 8, sizeof(*p) = 4
sizeof(ptr) = 8, sizeof(*ptr) = 20
*/
```

### &#x20;**Subscripting Pointer to an Array**

Suppose arr is a 2-D array with 3 rows and 4 columns and ptr is a pointer to an array of 4 integers, and ptr contains the base address of array arr.

```c
int arr[3][4] = {{10, 11, 12, 13}, {20, 21, 22, 23}, {30, 31, 32, 33}};
int (*ptr)[4];
ptr = arr;
```

![](.gitbook/assets/ptr\_to\_arr2.png)

Since ptr is a pointer to an array of 4 integers, ptr + i will point to ith row. On dereferencing ptr + i, we get base address of ith row. To access the address of jth element of ith row we can add j to the pointer expression _(ptr + i). So the pointer expression_ (ptr + i) + j gives the address of jth element of ith row and the pointer expression _(_(ptr + i)+j) gives the value of the jth element of ith row.&#x20;

We know that the pointer expression _(_(ptr + i) + j) is equivalent to subscript expression ptr\[i]\[j]. So if we have a pointer variable containing the base address of 2-D array, then we can access the elements of array by double subscripting that pointer variable.

```c
// C program to print elements of a 2-D array
// by scripting a pointer to an array
#include<stdio.h>
 
int main()
{
  int arr[3][4] = {
                    {10, 11, 12, 13},
                    {20, 21, 22, 23},
                    {30, 31, 32, 33}
                  };
  int (*ptr)[4];
  ptr = arr;
  printf("%p %p %p\n", ptr, ptr + 1, ptr + 2);
  printf("%p %p %p\n", *ptr, *(ptr + 1), *(ptr + 2));
  printf("%d %d %d\n", **ptr, *(*(ptr + 1) + 2), *(*(ptr + 2) + 3));
  printf("%d %d %d\n", ptr[0][0], ptr[1][2], ptr[2][3]);
  return 0;
}
/*
Output 
------
0x7ffead967560 0x7ffead967570 0x7ffead967580
0x7ffead967560 0x7ffead967570 0x7ffead967580
10 22 33
10 22 33
*/
```

## C Specification Full Reference

In case you need to know more about a certain concept, you may refer to C Specification which is openly available for reference.

Download here:

{% file src=".gitbook/assets/C Specification.pdf" %}
C Specification
{% endfile %}
