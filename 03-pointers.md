---
description: Pointers, the basics. The most important topic!
---

# 03 - Pointers

## What, why is the memory structure important?

**Memory Structure**

1. Code segment
2. Global storage
3. Stack
4. Heap

### 01 Code Segment

* **Read-only.**
* Consists of a sequence of **functions**.
* Each **function** is a sequence of machine code **instructions**.
* Each **instruction** occupies one or more bytes.
* The **last** instruction is the **“return” instruction.**

### 02 Global Storage

* **Global variables**
* Local **static variable**
* **String literals** "%d\n", "abc", "Hello"

### 03 Stack

* **Limited** amount of **memory**. _(stack overflow error could occur if exceeds)_
* **“Last in, first out”**
* Used to store:\

  * **Temporary/intermediate** calculation **results**
  * **Local variables** (except static variables)
  * Function parameters (sometimes)
  * **Function return location** (i.e. where to jump back to when a function finishes)

### 04 Heap

* **A giant pool** of **dynamically**-allocated **memory**.
* No particular fixed size or structure.
* **Allocated** and **freed** as needed by the program.

## What are pointers in C?

* **Each byte** in memory **has** a **unique**, **sequential “address”.**
* Where a value occupies multiple bytes (as most do), a **pointer** will **point to the first byte** (i.e. lowest memory address).
* **Pointers are a data type**, or rather a set of data types.
* **To declare a pointer**, we must also **declare what** sort of **data it points to**.
* A **pointer to an int** (`int*`) and a **pointer to a float** (`float*`) are **different data types**.

```c
/* The * is part of the data type, but attaches to one name only. */

float* ptr, number; /* ptr is a pointer, but number is just a float. */

number = 10;
ptr = &number;

/* The * has another meaning: “dereference”. */
printf("%d", (*ptr));
```

The pointer variable (`float* ptr`) can store an address of another variable (`float` number).

* If the variable (`float` number) it points to is a regular variable (non-pointer), then pointer (`float*` ptr) is a typical 1 level pointer (one \*)
* If the variable it points to is another pointer variable, then the pointer must be a double-pointer. 2-level pointer. (`float**`)

```c
float* ptr, number; 
float** doublePtr;

number = 10;
ptr = &number; /* ptr points to a regular variable */
doublePtr = &ptr; /* doublePtr points to another pointer variable */

printf("%d", *ptr); /* 10 */
printf("%p", (void*) *doublePtr); /* prints the value of ptr variable which 
is infact the address of the regular variable number */
```

### What is the size of a pointer?

Remember pointers are just like variables but holds a memory address. In a 64-bit O/S, the memory address takes 64 bits (accessible memory is $$2^{64}$$ Bytes) while in a 32-bit O/S, the memory address takes 32 bits (accessible memory is $$2^{32}$$ Bytes). Therefore:

```c
char* pcNum; /* sizeof(pcNum) = 8 Bytes => 64 bits */
int* piNum; /* sizeof(piNum) = 8 Bytes => 64 bits */
double* pdNum; /* sizeof(pdNum) = 8 Bytes => 64 bits */  
float* pfNum; /* sizeof(pfNum) = 8 Bytes => 64 bits */
int** ppiNum; /* sizeof(ppiNum) = 8 Bytes => 64 bits */
<any pointer type> ... /* sizeof(...) = 8 Bytes => 64 bits */
```

### How to declare multiple pointers on a single line?

```c
int *p1, *p2;
```

This declares the two pointers on a single line. Notice that there is an asterisk (`*`) for each pointer, in order for both to have type `int*` (pointer to int). This is required due to the operator precedence rules. Note that if, instead, the code was:

```c
int *p1, p2;
```

`p1` would indeed be of type `int*,` but `p2` would be of type `int`. Spaces do not matter at all for this purpose. But anyway, simply remembering to put one asterisk per pointer is enough for most pointer users interested in declaring multiple pointers per statement. Or even better: use a different statement for each variable.

## What is NULL?

* A special pointer value — a **"pointer to nothing"**.
* Useful for **initialising pointers**.
* Definition of null is compiler dependant. For GCC is it defined as`#define NULL ((void*) 0)`in **stddef.h** header\
  _(look at the preprocessed C code to see NULL definition)_

### stddef.h ?

* stddef.h is included in other standard headers such as stdio.h, stdlib.h, etc
* stddef.h mostly declares compiler things, so it comes with the compiler.&#x20;
* May have different definitions if compiling in 32-bit or 64-bit mode
* For instance, and you'll get different definitions with the compilers e.g. GCC and Clang.&#x20;
* Look for it in compiler directories. (use unix `locate` command)

## What are **void\* Pointers?**

* `void` pointer can point to any variable type (`char`, `int`, `double`, etc)
* **any single pointer type** (`char*`, `int*`, `double*`, etc) can be **assigned to** a **void\* pointer**
* **void\* pointer** can be **assigned to** **any single pointer type** (`char*`, `int*`, `double*`, etc) **or double pointer types** (`char**`, `int**`, `double**`, etc)

```c
int* piPtr;
int** ppiPtr;

void* pvPtr = piPtr;
piPtr = pvPtr;
ppiPtr = pvPtr;

/* Recall NULL which is defined as ((void*) 0)
NULL can be assigned to any single or double pointer type without 
explicit type casting */
```

## What are L-values and R-Values?

In an assignment, L-value is the left side value, r-value is the right side value. Not everything in C can appear as L-values. There are certain things that can appear as L-values and R-value.

**L-value** must indicate a **memory location** such as:

* a variable (a place to store some value)
* an expression that evaluates to a memory location

**R-value** is basically **some value** to be assigned to the L-value. R-value could be:

* a variable (the value of the variable will be read in this case)
* an expression (results in some value or evaluates to a memory location which has a value)&#x20;

{% tabs %}
{% tab title="R-Value" %}
```c
int num1, num2;
num1 = 10; /* R-value : is the value 10 */
num2 = 10 + 1; /* R-value : an expression that evalutes to 11 */
num2 = num1; /* R-value : is a variable where a value can be read from */
num2 = num1 + 1; /* R-value : an expression that evalutes to 11 */
```
{% endtab %}

{% tab title="L-Value" %}
```c
int num1, num2;
int* piNum;
int piArr[10];

num1 = 10; /* L-Value : a variable, a memory location which can store a value */

20 = 10; /* ERROR: L-value is not a memory location */
num1 + 20 = 10; /* ERROR: L-value is not a memory location. L-value expression 
evaluates to some value, but not a memory location. */

piNum = &num2; /* L-Value : a pointer variable, a memory location which can 
store a value. Here the value is the memory address of num2 */

*piNum = 20; /* L-Value : an expression that evaluates to a memory location.
this is the same location referenced by num2 variable as well */
printf("%d", num2); /* prints 20 */

*(piArr + 1) = 30; /* L-value : an expression that evaluates to a memory location.
*(piArr + 1) refers to the piArr[1] memory location in the array.
Refer Arrays & String Lecture for more details. */
```
{% endtab %}
{% endtabs %}

## What is the significance of size\_t type?

* Guaranteed to be big enough to contain the size of the biggest object the host system can handle.
* The maximum permissible size is dependent on the compiler it can be any of:
  * unsigned char
  * unsigned short
  * unsigned int
  * unsigned long
  * unsigned long long
* **size\_t** is **defined in stddef.h** as:
  * **typedef** long unsigned int **size\_t**;
* `sizeof(...)` returns `size_t` value
* Following sizes may change across different hardware (64 bit, 32 bit)
  * sizeof(char) == 1
  * sizeof(short) == 2
  * sizeof(int) == 4
  * sizeof(float) == 4
  * sizeof(double) == 8
  * sizeof(void\*) == 8
  * sizeof(short\*\*) == 8

## What is the use of malloc(...) / free(...)

* Heap allocation and de-allocation are done using void\* malloc(\<no\_of\_bytes>) and free(void\* ptr) functions
* malloc-ed memory is not associated with a name, unlike typical local variables.
* malloc does not care what type of data you are going to store. It simply returns a consecutive memory block of the size requested.&#x20;
* malloc-ed memory can be interpreted as int blocks, char blocks, etc by the respective pointer typecasting (int\*, char\*, etc)

```c
int main() 
{
    int num = 10; /* local variable. Stack memory allocated with the name num */
    int* piPtr = (int*) malloc(sizeof(int)) /* Heap memory allocated without a name */
    /* the malloc-ed memory above is interpreted as an integer block (4-byte block) */

    char* pcPtr = (char*) piPtr;
    /* the same malloc-ed memory is interpreted as char blocks block (1-byte blocks) */

    /* IMPORTANT: 
    Pointers piPtr, pcPtr, etc will always store the memory address of the first byte. 
    During the dereference,
    The compiler reads the address stored in the pointer.
    Goto the memory location.
    Depending on the pointer type (int*, char*, etc), it determines how many bytes to 
    look at and interpret as an int, char, etc.
    */
}
```

## Accessing memory after freeing it?

> I know I am NOT supposed to access the memory after I freed it. However, just to satisfy my curiousity, I tried to access the memory immediately after I freed it to see the error. I was expecting to see the \*Segmentation Fault\* error. For some reason, I did not see \*Segmentation Fault\* error. Why is that?

In short, accessing a freed memory block can result in undefined behaviour. You might OR might not get \*segmentation fault\* error. This behaviour is system-dependent. If you did not see the \*segmentation fault\* error, it means the Operating System is not yet aware that memory is already free (but it will eventually). That is why it is a good idea to assign NULL to the pointer variable immediately after you free it to prevent accidental access to it.

{% tabs %}
{% tab title="Bad habit after using free()" %}
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int * pointer = (int *) malloc(sizeof(int));
    *pointer = 10;
    
    free(pointer);
    
    /* If you forgot to set pointer to NULL after freeing it, you might accidentally
       access that memory again later on. */
    
    return 0;
}
```
{% endtab %}

{% tab title="Good habit after using free()" %}
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int * pointer = (int *) malloc(sizeof(int));
    *pointer = 10;
    
    free(pointer);
    pointer = NULL;
    /* It is a good habit to assign NULL into the pointer variable after you
       free it. This will prevent accidental unwanted access to it. */
    
    return 0;
}
```
{% endtab %}
{% endtabs %}

## Why do we need valgrind?

* valgrind can help you to find:
  * Using uninitialised values
  * Accessing unallocated memory
  * Failing to free memory
  * Freeing the same memory more than once
  * Losing track of allocated memory (memory leaks)
* **command:** valgrind \[options] ./program
  * **option:** `--leak-check=full` _(find where the leak occurred)_
  * **option:** `--track-origins=yes` _(controls whether Memcheck tracks the origin of uninitialised values)_
* `gcc` **-g** `-Wall -pedantic -ansi main.c -o prog`
  * Add extra debug information into the executable for valgrind:

## Why does my valgrind shows an extra allocation?

> Why does my valgrind shows an extra allocation with 1024 bytes OR 4096 bytes? I did not malloc anything else other than the one I use.

The most common reason is because you use printf() in your program. When you call printf(), the system will usually create an I/O buffer (which uses memory space) where it temporarily store the data, so that it can send them in bulk to the next stage for efficiency purpose. The size of the buffer depends on the platform OR operating system. That is why you might see extra memory usage such as 1024 allocated bytes or 4096 allocated bytes. The good news is that printf will free that I/O buffer, so you don't have to worry about memory leaks on that aspect.

{% tabs %}
{% tab title="Only 1 allocation from int malloc" %}
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int * pointer = (int *) malloc(sizeof(int));
    /* We allocate a memory block for one int in the heap.
       Normally, the size is 4 bytes */
    
    free(pointer);
    /* If you run valgrind on this program, It should mention "1 alloc"
       and "4 bytes allocated" */
    
    return 0;
}
```
{% endtab %}

{% tab title="Have an extra memory allocation from calling printf()" %}
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int * pointer = (int *) malloc(sizeof(int));
    /* We allocate a memory block for one int in the heap.
       Normally, the size is 4 bytes */
    
    *pointer = 100;
    
    printf("the number is %d\n", *pointer);
    /* This printf will allocate memory block for I/O buffer */            
    
    free(pointer);
    /* If you run valgrind on this program, It should mention "2 allocs"
       and you will see extra memory allocated such as 1024 bytes OR 4096 bytes
       from the printf itself. The extra memory size depends on the system
       you are running this program on. */
    
    return 0;
}
```
{% endtab %}
{% endtabs %}

## Data Types in C

Data Types in C mainly falls into the following hierarchy

* Arithmetic Types (fundamental types)
  * Integer Types (int, char)
  * Floating Point Types (float, double)
* Derived Types (derived from fundamental data types)
  * Pointers
  * Function Pointers
  * Arrays
  * Structures

### Type Modifiers

Type modifiers can be used to alter the data storage of a data type. For _some_ of the fundamental data types:

```c
unsigned int uiNum; /* sign bit is not used to maintain the sign of the number. 
Therefore can only store positive numbers */

int iNum; /* same as signed int */
signed int iNum2; /* here `signed` is optional */

unsigned char cChr;
signed char cChr2;

short int siNum; /* half the size of a typical integer. 16 bits */

long int liNum; /* doubles the amount of bits for an integer. 64 bits */
unsigned long int uliNum;

long long int llNum;

...
...

/* REMEMBER: not all type modifiers are applicable to all types.
Also, the behavior the type modifier are highly platform dependant. */
```

You may refer to: for more details. Please do check the size of the data types using `sizeof(...)` and also make sure your code is C89 compliant by compiling it using `gcc -ansi -pedantic`

{% embed url="https://www.programiz.com/c-programming/c-data-types" %}

{% embed url="https://www.geeksforgeeks.org/data-types-in-c/" %}

## What are Literals?

Literals are **valid** values.

* 2 is an int literal
* 2U is an unsigned int literal
* 2L is a long literal
* 2LL is a long long literal
* 2.0f is a float literal
* 2.0 is a double literal
* ‘A' is a char literal

### Is it possible to have a pointer literal?

> In C one can have string literals in the form of
>
> ```c
> char* zStr = "string here"; /* "string here" is the string literal */
> int iNum = 5; /* 5 is the integer literal */
> long lNum = 90322L; /* 90332L is the long literal */
> float fDecimal = 6.3f; /* 6.3f is the float literal */
> double dDecimal = 6.0; /* 6.0 is a double literal */
> ```
>
> Is the a way to have a pointer literal? That is a literal address to a memory space.&#x20;
>
> ```c
> int* source = 0x08000000;
> ```

&#x20;No, it's not. That is because literals are **valid values**, and the only valid pointers are addresses of objects, i.e. the **result** of address-of operations or of **pointer arithmetic** on valid pointers.

Exception: In C, the **only** pointer literal or constant is zero which is also casted with (void\*) to define the `NULL` constant.&#x20;

i.e. `#define NULL (void*) 0`

Read [What is NULL?](https://curtin-ucp.gitbook.io/faq/02-environments/what-is-null)

### Why are pointers to literals not possible?

> Is this valid?
>
> ```c
> int* pInt = &5; /* address of a literal */
> ```

A literal **does not** occupy any memory. A literal just exists.

This makes that there is **no address** for a pointer to refer to when it would point to a literal and for that reason, pointers to literals are forbidden.

## What is pass by value and pass by reference?

Pass by value, is basically copying the argument value to the function parameter when a function is invoked.

Pass by reference, is basically copying a reference to a variable to the function parameter when a function is invoked. Basically, the value of a pointer (memory address) is copied to the function parameter when a function is invoked. i.e. refer to the code below

### What is a function frame:

In the example below, when testFunc() is invoked inside main() function, the testFunc() function frame will be stacked on top of main() function frame in the stack region of the memory. (Remember, memory has 4 regions: stack, heap, global, code)

A function frame in the stack includes allocations for all local variable definitions and the parameters defined in the function.

```c
#include <stdio.h>

void testFunc(int iNum1, int* piNum2)
{   /* iNum1 will be 0, piNum2 will be Ox00000A1 */    
    /* iNum1 and piNum2 are both local variables to testFunc */
    /* But, piNum2 is a pointer that holds an address to a variable
    defined in main() function frame (iNumber2) */
    
    iNum1 = 10; /* we change the local parameter value */
    *piNum2 = 10; /* We change the pointed value (iNumber2's value) */
}

int main()
{
    /* Assume address of iNumber2 is 0x00000A1 */
    int iNumber1 = 0, iNumber2 = 0;
    testFunc(iNumber1, &iNumber2); /* here we pass the arguments as below */ 
          /*( 0      , 0X00000A1) */
          
    printf("%d %d", iNumber1, iNumber2); /* prints, 0 10 */
  
    return 0;
}
```

