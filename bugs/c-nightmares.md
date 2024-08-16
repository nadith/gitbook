---
description: Content for C-Gurus
---

# C Nightmares

Feel free to research the following issues... :) Answers will be discussed during the tutorial sessions towards the end of the semester. These questions need in-depth understanding of C language along with the operations of `gcc` compiler and sometimes the optimizations performed by the compiler itself. This is purely for the students who are interested to know more in-depth concepts.

## Why is possible to assign an int variable to a character variable without casting!

> integer variable occupy 32 bits while character variable only occupy 8 bits (platform dependant). Hence gcc allows you to go from bigger type to smaller type without explicit casting. No warning or no errors. What, Seriously!  ... :D See whether you can crack the problem and find out the answer
>
> ```c
> int iNum = 500;
> char ch = iNum;
> /* Program compiles successfully */
> ```

## Is it possible to define the following set of pointers?

> 01\. A pointer points to itself?\
> 02\. A double pointer points to itself?\
> 03\. A triple pointer points to itself?

## Why is the following double pointer to pointer assignment is allowed? What happens during dereference?

> ```c
> int iNum = 10;
> int* piNum = &iNum;
> int* piNum2 = NULL;
>
> int** ppiNum = &piNum;
> piNum = (int*) ppiNum;
> piNum2 = (int*) ppiNum;
> ```
>
> 1. what happens if `piNum, piNum2` is dereferenced?
> 2. Is `piNum, piNum2` allowed to be double dereferenced or single dereferenced? Justify your answer?
> 3. Consider the following program:
> 4. ```c
>    #include <stdio.h>
>
>    int main()
>    {
>        int iNum = 10;
>        int* piNum = &iNum;
>        int* piNum2;
>        unsigned long* pulNum;
>
>        printf("%d: %p\n", *piNum, (void*) piNum);
>       
>        int** ppiNum = &piNum;
>        printf("%d: %p : %p\n", *((int*)*ppiNum), ((void*)*ppiNum), (void*)ppiNum);
>
>        pulNum = (unsigned long*) ppiNum;
>        printf("  : %p : %p\n", (void*)pulNum, (void*)ppiNum);
>        printf("  : %p : %p\n", (void*)*pulNum, (void*)*ppiNum);
>        printf("  : %d : %d\n", *((int*)*pulNum), **ppiNum);
>
>        piNum2 = (int*) ppiNum;
>        printf("  : %p : %p\n", (void*)piNum2, (void*)ppiNum);
>        printf("  : %p : %p\n", (void*)*piNum2, (void*)*ppiNum);
>        printf("  : %d : %d\n", *((int*)*piNum2), **ppiNum);
>
>        return 0;
>    }
>
>    /* 01: Output is: 
>
>    10: 0x7ffd060e91d4
>    10: 0x7ffd060e91d4 : 0x7ffd060e91d8
>      : 0x7ffd060e91d8 : 0x7ffd060e91d8
>      : 0x7ffd060e91d4 : 0x7ffd060e91d4
>      : 10 : 10
>      : 0x7ffd060e91d8 : 0x7ffd060e91d8
>      : 0x60e91d4 : 0x7ffd060e91d4
>    Segmentation fault (core dumped)
>    */
>
>    /* 02: During Compilation */
>    /* Warnings: 
>
>    main.c:12:5: warning: ISO C90 forbids mixed declarations and code [-Wdeclaration-after-statement]
>         int** ppiNum = &piNum;
>         ^~~
>    main.c:22:29: warning: cast to pointer from integer of different size [-Wint-to-pointer-cast]
>         printf("  : %p : %p\n", (void*)*piNum2, (void*)*ppiNum);
>                                 ^
>    main.c:23:31: warning: cast to pointer from integer of different size [-Wint-to-pointer-cast]
>         printf("  : %d : %d\n", *((int*)*piNum2), **ppiNum);
>
>    */
>    ```
>
> > 1. Explain the output in detail? Provide solid reasons behind each line of ouput including the reason for seg-fault
> > 2. Explain the warnings in detail?

## What is the difference between using %x and %p format specifiers?

> What is the difference between using %x and %p format specifiers in printf to print a pointer value or an integer value? i.e.
>
> ```c
> int iNum = 15;
> int* piNum = &iNum;
> printf("%x", iNum); /* ?? */
> printf("%x", piNum); /* ?? */
> printf("%p", (void*) iNum); /* result in a warning, explain why */
> printf("%p", (void*) piNum); /* ?? */
> ```

## Can you access elements in 2d array using a 1d pointer, but not a double pointer?

> Have a look at the following program. How can you use a 1d pointer to access all the elements in a 2d array?
>
> ```c
> char arr2dCh[3][4] = {"ABC", "CDE", "EFG"};
> printf("%c\n", arr2dCh[0][0]); /* `A` */
>
> char* arrCh = &(arr2dCh[0][0]);
> printf("%c\n", arrCh[0]); /* `A` */
> /* how to use arrCh to print all the elemnts of arr2dCh? */
> ```

## Malloc nightmares!

### Can you malloc and use sizeof(...) operator to get the size of the actual malloc?

> Modify the program below to get the size of the malloc using sizeof(...) operator. How to enforce the compiler to know the no. of bytes allocated in the heap prior to the runtime ?
>
> ```c
> char* arrCh;
> arrCh = (char*) malloc (sizeof(char) * 15);
> printf("%ld", sizeof(arrCh)); /* prints 8 */
> /* needs to print 15 */
> ```

### Can you access elements in 2d malloc-ed array using a 1d pointer?

> Can you use a 1d pointer to access all the elements in a 2d malloc array? If not, explain your answer
>
> ```c
> /* Assume arr2dCh is a 2d malloc array for 3 by 4 */
>
> char* arrCh = &(arr2dCh[0][0]);
> /* how to use arrCh to print all the elemnts of arr2dCh? */
> ```

### Why does the following produce a warning?

> ```c
> char (*c)[5] = (char *)malloc(5);
> ```

### Can you declare a 2d malloc-ed array that resembles a typical static array memory layout?

> with a static array declaration such as `int arr2d[3][4]` , you can access the $$2^{nd}$$ element of the second row using the $$1^{st}$$ row pointer as `arr2d[0][5]`
>
> Can you declare a 2d malloc-ed array that could do the same?

### Is it possible to partially free a malloced array?

> Consider the following code. Can you partially free a malloced array?
>
> ```c
> int* arriNums = (int*) malloc(sizeof(int) * 10);
> int* piNums = arriNums + 5;
> free(piNums) /* What's going to happen here?, free only 5 elements? */
> ```

{% hint style="info" %}
&#x20;You must `free` every `malloc`'ed string exactly once. Calling `free` several times on the allocated memory will lead to undefined behaviour.
{% endhint %}

## Can you return a static function in a .c file via a function pointer?

> static functions are private to the .c file where it is defined. But can you return a pointer to a static function to other .c files (i.e. to main.c ) so that you can use the static function via the function pointer?

## Compilation / Makefile nightmares

### Can you compile .txt file? does the extension matter?

> Assume you have some C code saved in the main.txt (.txt extension). Is it possible to compile it using `gcc -Wall -ansi -pedantic main.txt -o prog`? Is it a mandotory requirement to have .c or .h extensions for compilation? If not, what is the advantage of naming the files with .c or .h extension?

### Is it possible to generate object files with a custom file name?

> consider the compilation:  `gcc -Wall -ansi -pedantic main.c -c`
>
> It will generate main.o file. Later you can use the linker to generate the executable file (`gcc main.o -o prog`)
>
> Lets say, you want your generate object file  to have a different name other than main.o (i.e. ucp.ex). Is this possible?

### Can the custom header be included using < > brackets rather than using " " quotes?

> Yes  you can.&#x20;
>
> ```
> $ gcc -Iheaders ...
> ...
> #include "..." search starts here:
> #include <...> search starts here:
>  headers
>  /usr/local/include
>  /usr/lib/gcc/x86_64-linux-gnu/4.4.5/include
>  /usr/lib/gcc/x86_64-linux-gnu/4.4.5/include-fixed
>  /usr/include/x86_64-linux-gnu
>  /usr/include
> ```
>
> Ref: [https://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art026](https://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art026)

