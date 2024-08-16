---
description: How to be a professional coder.
---

# Coding Standards

## Why do coding standards/guidelines matter?

Coding guidelines define a standard for the look and feel of your code. These guidelines are a set of rules describing how your code should look, which features of the programming language you will use, and how. The important thing isn’t which set of conventions is better, but rather to have standard and use it consistently.&#x20;

These guidelines will not only help you in reading code from your instructor and classmates but will help your tutors as they read (and grade!) your code. These guidelines should make you a more productive programmer because you do not have to make decisions about trivial matters, you can spend your energy on the solution of real problems. It is also important to remember that most software firms follow strict coding standards, and this is one of the ways to simulate the real world as closely as possible. You are expected to conform to these Coding Guidelines in all your programming.&#x20;

Deviations from this standard are acceptable if they **enhance readability and code maintainability. Major deviations required an explanatory comment** at each point of departure so the person assessing your code will know that you didn’t make a mistake, but purposefully are doing a local variation for a good cause.

If you ignore these guidelines, then your code will be **penalized** accordingly.

## Can I use single-letter variables?

You are not allowed to use single-letter variable names since it badly affects the readability of the code. You may use single-letter variable names **only for** index variables. In all other cases, use meaningful variable names.

```c
int count = 0;
scanf("%d", &count);

int i; /* i is an index variable */
for (i=0 ; i < count ; i++)
{
    printf("Num %d\n", i);
}
```

## Is there a smart way to name the variables?

One of the methods to name your variables so that it helps you during debugging is to follow a naming pattern as below:&#x20;

_Use a prefix in the identifier name to indicate what type of variable it is. This is particularly helpful when referring to the variable (especially pointer variables) somewhere in the middle of the code where the type is not directly visible unless you refer back to the place where it is declared._

```c
int iNum; /* iNum is an integer */
float fNum; /* fNum is a float */
double dNum; /* dNum is a double */

int* piNum; /* piNum is a pointer to an integer */
int* piNums; /* piNumsis a pointer to an integer array - dynamic allocation */
int aiNumbers[10]; /* or */ int arriNumbers[10];
/* aiNumbers or arriNumbers is a static array of ints */

float* pfNum; /* pfNum is a pointer to a float */
float* pfNums; /* pfNums is a pointer to a float array) */

int** ppiNum; /* ppiNum is a double pointer to an integer */
int** ppiNums; /* ppiNums is a double pointer to an integer array - dynamic allocation */

char* pcLetter; /* pcLetter is a pointer to a char */
char* pcLetters; /* pcLetter is a pointer to a char array - dynamic allocation */

char* zName; /* z denotes null terminated char array (c-string). 
                Therefore, zName is a pointer to a null-terminated 
                char array */
char* zName[10]; /* zName is a pointer to a null-terminated char array */

/* Function Pointers */
void (*pfnGetDate)();   /* pfnGetDate is a pointer to a function with the template void <func_name>() */
int (*pfnAdd)(int,int); /* pfnAdd is a pointer to a function with the template int <func_name>(int, int) */

/* Ref - (13 - C Advanced) Section */
int (*pai10Nums)[10]; /* pai10Numsis a "pointer to an array of 10 ints" */
int (*pai5Nums)[5];   /* pai5Nums is a "pointer to an array of 5 ints" */

int (*(*pfnIamCrazy)(int, int))[5]; 
/* pfnIamCrazy is a pointer to function with the template 

int (*)[5] <func_name>(int, int);

Note, int (*)[5] is the return type of the function and 
needs to be typedef-ed */

int (*(*(*pfnIamSuperCrazy)(int, int)))[5]; 
/* pfnIamSuperCrazy is a pointer to function with the template 

int (*(*))[5] <func_name>(int, int);

Note, int (*(*))[5] is the return type of the function and 
needs to be typedef-ed */


```

For more information please refer to:

{% embed url="https://en.wikipedia.org/wiki/Hungarian_notation" %}

## Do I need to format the code (whitespaces, Tabs)?

You must always format your code with necessary tabs, whitespaces to enhance the code's readability (Ref: Coding Standard Document).&#x20;

{% tabs %}
{% tab title="Bad Formatting" %}
```c
int count=0; /* no whitespaces */
int i;
for(i=0;i<count;i++) { /* no whitespaces, start curly brace on next line */
printf("%d", i);/* no indentation */
}
```
{% endtab %}

{% tab title="Good Formatting" %}
```c
int count = 0;
int i;
for(i = 0 ; i < count ; i++) 
{
    printf("%d", i);
}
```
{% endtab %}
{% endtabs %}

## Best Practices for Pointers - Recommended in UCP

* Always **initialize pointers** to **NULL** at the declaration
* Always **free** the memory **if malloc**-ed and **assign NULL** to the **pointer** after free

```c
int main() 
{
 /* Use the prefix "pi", "pc", "pd", etc for int*, char*, double* pointer names */
 /* Use the prefix "ppi", "ppc", "ppd", etc for int**, char**, double** pointer 
 names */

 int* piNum = NULL; /* initialize to NULL at the declaration */
 piNum = (int*) malloc (sizeof(int));
 ...
 ...
 free(piNum);
 piNum = NULL; /* Always assign NULL after free(...) */

 return 0;
}
```

## Best Practices for Malloc - Recommended in UCP

* If malloc is used inside a function, always try to free it before you exit the function.
  * inside a function, whenever you write the malloc(...) statement, write the corresponding free(...) statement and then you can put your code in between malloc and free. In this way, no memory leaks due to the lack of free(...) will appear in your program.
  * ```c
    void testFunc()
    {
      int* piNum = (int*) malloc (sizeof(int));

      ...
      /* Place your code here */
      ...
      
      free(piNum);
      piNum = NULL; /* Always assign NULL after free(...) */
      return 0;
    }
    ```
* On occasions where free(...) is impossible inside a function due to the malloc-ed memory has to be used outside the function, follow the steps below:
  * Writes the functions allocMemory(...), freeMemory(...) and take as many parameters (pass by reference) to allocate and return memory. You may change the function names allocMemory, freeMemory to meaningful names depending on the context.
  * In the parent function (i.e. main()) call allocMemory(...) with parameters which you want to be allocated.
  * In the parent function (main()), call freeMemory(...) with the parameters which you have used to call allocMemory(...). This will free the malloc-ed memory.

{% tabs %}
{% tab title="main" %}
```c
int main(int argc, char** argv)
{
    int* piNum1, piNum2, piNum3;
    initEnvironment(&piNum1, &piNum2, &piNum3, argc, argv); /* Allocate memory here */
    /* After the initEnvironment() call, piNum1, piNum2, piNum3
    will be initialized to NULL or malloc-ed memory. 
    See the implementation for details */
   
    ...
    /* Place your code here */
    ...

    destroyEnvironment(&piNum1, &piNum2, &piNum3); /* Destory memory here */    
    /* After the destroyEnvironment() call, piNum1, piNum2, piNum3
    will be set to NULL.
    See the implementation for details */    
    
    return 0;
}

/* If the paramater list becomes too long, pack it to a C-struct and pass the 
struct to the initEnvironment(...) and destoryEnvironment(...) functions */ 

```
{% endtab %}

{% tab title="initEnvironment" %}
```c
void initEnvironment(int** ppiNum1, int** ppiNum2, int** ppiNum3, int argc, char** argv) 
{
    *ppiNum1 = NULL;
    *ppiNum2 = NULL;
    *ppiNum3 = NULL;
    
    if (some_logic == TRUE) /* You may validate command line arguments, etc. If successful, */
    {
        /* allocate memory and make it accessible to the caller
        via pass by ref parameters */
        *ppiNum1 = (int*) malloc(size(int));
        *ppiNum2 = (int*) malloc(size(int));
        *ppiNum3 = (int*) malloc(size(int));
    }
    
    /* Remember, assigned NULL or allocated memory is 
    accessible to the caller via pass by ref parameters */    
}
```
{% endtab %}

{% tab title="destroyEnvironment" %}
```c
void destroyEnvironment(int** ppiNum1, int** ppiNum2, int** ppiNum3) 
{
    if (*ppiNum1 != NULL) /* condition is optional since free(NULL) will not cause a problem */
        free(*ppiNum1);
    
    if (*ppiNum2 != NULL) /* condition is optional since free(NULL) will not cause a problem */
        free(*ppiNum2);
    
    if (*ppiNum3 != NULL) /* condition is optional since free(NULL) will not cause a problem */
        free(*ppiNum3);    
}
```
{% endtab %}
{% endtabs %}

## Best Practices for Commenting

Your code should always have some good set of comments. Not too long, not too short. Probably a single line that explains the main idea of the code segment is optimal.&#x20;

{% hint style="info" %}
**UCP Recommendation:** Use function header comments (**doc comments** with the format shown below) for core functions and **code comments** for the code inside functions. For short utility functions, you may choose to write/avoid function header comments.
{% endhint %}

Using meaningful variable names and function names will avoid the need for explicit commenting! Look at a good example, the variable names speak themselves.

{% tabs %}
{% tab title="Good Example" %}
```c
#include <stdio.h>

/**
 * @brief  Adds two numbers
 * @note   Adds two numbers and returns the summation.    
 * @param  iNum1: Number 01
 * @param  iNum2: Number 02
 * @retval int: summation
 */
int add(int iNum1, char iNum2)
{
    /* adds the numbers */
    return iNum1 + iNum2;
}

/**
 * @brief  Entry point of the program 
 * @note   Entry point of the program; defines the main flow of the program.
 * @retval int: success status
 */
int main()
{
    /* Note: No need to comment the following code, since 
    the code speaks itself */
    
    int sum = add(10, 20);
    printf("Sum: %d\n", sum);
    return 0;
}
```
{% endtab %}

{% tab title="Bad Example" %}
```c
#include <stdio.h>

/* Add two numbers */ /* Note: <- not properly formatted doc-comment! */
int add(int iNum1, char iNum2)
{
    /* NOTE: Following comment is way too long. Best is, short single line comment! */
    
    /* accepts two integer numbers and add those numbers, return the summation. 
    Summation is an integer. If numbers are negative, result will be negative.
    If the numbers are positive, result will be positive. */

    return iNum1 + iNum2;
}

/* main function */ /* Note: <- not properly formatted doc-comment! */
int main()
{
    int sum = add(10, 20);
    printf("Sum: %d\n", sum);
    return 0;
}
```
{% endtab %}
{% endtabs %}

Avoid too many code comments since it will affect the readability of the code. After all, comments are there to improve the readability of the code but not to destroy it!

For if conditions, loops, etc: just write a 1 or 2 line code comment to brief the main logic. Write a code that speaks itself.

**Don't forget to indent the code properly!**

Once the doc comments are done with the format shown above, VS Code IntelliSense will read and display it nicely _(press Ctrl + hover mouse over the function call add in main() function)_

![Install \`C-family Documentation Comments\` Plugin to VS Code for easy doc-comment generation.](<../.gitbook/assets/doc comments.JPG>)

## Best Practices for if, while, etc control structures (Cyclomatic Complexity)

Cyclomatic complexity is a simple measure of complexity in an application or routine. It measures the paths through the code. Cyclomatic complexity is never less than 1, because there’s always at least one code path.

Consider the following code:

```c
int foo() 
{     
    if (Check#1)
    {
        CodeBlock#1
    }
     
    for(... ; Check#2 ; ...) 
    {         
        if (Check#3)
        {
            CodeBlock#3
        }
         
        if (Check#4)
        {
            CodeBlock#4
        }
        
        if (Check#5)
        {
            CodeBlock#5
        }         
    }
     
    return TRUE;     
}
```

Cylcomatic complexity measures the paths through the codebase. The function is the first entrance point, so it counts as 1. Each conditional or loop is another point. The total cyclomatic complexity of this function is 6; there are four if statements, one loop, and the one for the function itself.

Cyclomatic complexity is one measure of code quality. It helps us know exactly how complex a particular routine is, and helps us refactor that routine as necessary.&#x20;

#### Cyclomatic Complexity:

* below 4: considered good; **(Recommended in UCP)**
* between 5 and 7: medium complexity
* between 8 and 10: high complexity
* 10 above: extreme complexity.

### Nested if, loop, switch statements?

Avoid going beyond 3 sublevels (absolute max is 4), refactor your code into sub-functions.

{% tabs %}
{% tab title="Bad Example" %}
```c
if (Check#1)
{
    CodeBlock#1
    if (Check#2)
    {
        CodeBlock#2
        if (Check#3)
        {
            CodeBlock#3
            if (Check#4)
            {
                CodeBlock#4
                if (Check#5)
                {
                    CodeBlock#5
                    if (Check#6)
                    {
                        CodeBlock#6
                        if (Check#7)
                        {
                            CodeBlock#7
                        }
                        else
                        {
                            rest - of - the - program
                        }
                    }
                }
            }
        }
    }
}
```
{% endtab %}

{% tab title="Good Example" %}
```c
int condition = Check#1;
if (condition)
{
    CodeBlock#1
}

condition = (condition && Check#2);
if (condition)
{
    CodeBlock#2
}

condition = (condition && Check#3);
if (condition)
{
    CodeBlock#3
}

... 
/* if there are more if conditions, refactor into 
* sub functions. Always keep a low cyclomatic complexity.
*/
```
{% endtab %}
{% endtabs %}

## What is Code Quality? How to improve code quality?

{% embed url="https://www.perforce.com/blog/sca/what-code-quality-and-how-improve-code-quality" %}

## Best Practice for Header Files

**NEVER** define header files for each function that you write. Make sure to group a set of related functions into a header file. **Logical division** of functions of your program into meaningful header files is **highly recommended in UCP**.&#x20;

#### Bad Example:

{% tabs %}
{% tab title="main.c" %}
```c
#include <add.h>
#include <subtract.h>
#include <div.h>

int main()
{
    int iNum1 = 10, iNum2 = 5;
    printf("Addition: %d\n", add(iNum1, iNum2));
    printf("Subtraction: %d\n", subtract(iNum1, iNum2));
    printf("Division: %d\n", divide(iNum1, iNum2));

    return 0;
}
```
{% endtab %}

{% tab title="add.h" %}
```c
#ifndef ADD_H
#define ADD_H

int add(int iNum1, iNum2);

#endif
```
{% endtab %}

{% tab title="subtract.h" %}
```c
#ifndef SUBTRACT_H
#define SUBTRACT_H

int subtract(int iNum1, iNum2);

#endif
```
{% endtab %}

{% tab title="divide.h" %}
```c
#ifndef DIVIDE_H
#define DIVIDE_H

int divide(int iNum1, iNum2);

#endif
```
{% endtab %}
{% endtabs %}

**Good Example:**

{% tabs %}
{% tab title="main.c" %}
```c
#include <calculator.h>

int main()
{
    int iNum1 = 10, iNum2 = 5;
    printf("Addition: %d\n", add(iNum1, iNum2));
    printf("Subtraction: %d\n", subtract(iNum1, iNum2));
    printf("Division: %d\n", divide(iNum1, iNum2));

    return 0;
}
```
{% endtab %}

{% tab title="calculator.h" %}
```c
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int iNum1, iNum2);
int subtract(int iNum1, iNum2);
int divide(int iNum1, iNum2);

#endif
```
{% endtab %}
{% endtabs %}
