---
description: Basics of C programming. No pointers!
---

# 01 - C Basics

## What are the differences between C89 and C99?

Please refer to the Wiki Page for Practical 1 for some of the examples.

## What are variable declarations, definitions, initializations, assignments, function declarations, definitions, invocations? What other terminology is there in C programming?

Please refer to the "Glossary" in Blackboard for the terminology that you need to be aware of in C programming. **Getting the right terminology is very important** in UCP to understand the advanced concepts of C programming in the later weeks. So make sure you know the terms right!

## What happens if a variable is declared but not initialized? Any implications?

During variable declaration, the variable will be allocated memory depending on the variable type (In a 64-bit O/S,  int - 32 bits, float - 32 bits, double - 64 bits, etc). Since the variable is not initialized, the value it would contain is the value that already exists in the location where the memory is allocated. This is some random garbage value.&#x20;

## Do I need to know function recursion?

> I'm not from a programming background, but I do have a fair bit of understanding of programming constructs such as if, while, etc. I find it difficult to understand function recursion to perform the lab practical? is it really necessary?

No, it's not mandatory to know function recursion as long as you know the control structures such as if, while, do-while well in C programming. Function recursion is an alternate way to perform certain tasks easier and faster than writing a while loop.

## **Difference between int main() and void main()?**

Like any other function, main is also a function but with a special characteristic that the program execution always starts from the ‘main’. ‘int’ and ‘void’ are its return type. So, let’s discuss all of the three one by one.

void main – The ANSI standard says "no" to the ‘void main’ and thus using it can be considered wrong. One should stop using the ‘void main’ if doing so.

int main – ‘int main’ means that our function needs to return some integer at the end of the execution and we do so by returning 0 at the end of the program. 0 is the standard for the **“successful execution of the program”**. \
This is an indication to the underlying O/S which in fact calls the entry point (`main()` function) in your program.

main – In **C89**, the **unspecified return type** defaults to int. So, **the main is equivalent to int mai**n in **C89**. But in C99, this is not allowed and thus one must use int main. So, **the preferred way is int main.**

## **What are %d, %c, etc symbols?**

Those are called format specifiers for printf and scanf functions in C. Acts as a place holder and will be replaced with the actual value of the variable when used in printf(...). You will learn more about it in "Arrays and Strings" Lecture.

```c
int num = 10;
pritnf("%d", num); /* num is an integer */
```

Some of the format specifiers are:\
%d - int (same as %i) \
%ld - long int (same as %li) \
%lu - long unsinged int \
%f - float \
%lf , %g - double \
%c - char \
%s - string \
%x - hexadecimal

Reference:

{% embed url="https://en.wikibooks.org/wiki/C_Programming/Simple_input_and_output" %}

Also, refer to "Input / Output" lecture for more details.

## What happens if the user inputs a character when an integer (%d) is expected?

Basically, it will not set the character to the integer variable. The variable will be left untouched. Therefore it is important to use the correct placeholder (format specifier) for the right input data type.

{% tabs %}
{% tab title="Example 01" %}
```c
int num = 0; /* Initialize the variable to 0 */
scanf("%d", &num); /* if user inputs character 'A' */
print("%d", num); /* num will still be 0 */
```
{% endtab %}

{% tab title="Example 02" %}
```c
int num; /* num contains some garbage value = 42098 */
scanf("%d", &num); /* if user inputs character 'A' */
print("%d", num); /* num will still be 42098 */
```
{% endtab %}
{% endtabs %}

## Why do we need to use \&variable\_name in scanf but not in printf?

Variable has 4 important parts.

1. Memory allocation (depends on the variable type)
2. Memory address where the memory allocation is
3. Name (alias to refer to the allocated memory)
4. Value

`&` sign will fetch the memory address of a variable given the variable name. Since scanf stores a value from the user in a variable, it needs to know the actual memory address of the variable to locate the location in the memory.

```c
int num;
char ch;
scanf("%d", &num);
scanf(" %c", &ch); 
/* make sure to keep a space " %c" to consume all the whitespaces 
preeding the user input character. In contrast, "%d" consumes all 
whitespaces by default.

Ref: https://eecs.wsu.edu/~cs150/reading/scanf.htm
*/
```

{% embed url="https://eecs.wsu.edu/~cs150/reading/scanf.htm" %}

printf simply prints the value of a variable to the terminal. Therefore it does not need a memory address of the variable but just the alias (variable name) to refer to the value stored in the variable.

```c
int num = 10;
pritnf("%d", num); 
/* num is the variable name (alias to the allocated memory) and
when used, the value store in the memory location will be read. */
```

You will learn about memory address, `&` sign in detail in "Pointers" lecture.

## Why "%c" needs a space " %c" unlike "%d" in scanf?

To consume the whitespace in front of %c. This is because whitespace is also treated as a valid character. In the case of %d, it consumes whitespaces by default.

Please refer to: for more details:

{% embed url="https://eecs.wsu.edu/~cs150/reading/scanf.htm" %}

i.e.

```c
Ask for int:
buffer: 1\n
int is read %d
buffer: \n
ask for char
buffer: \na\n
read char or string, \n is read
buffer: a\n (left in buffer)
```

## **gcc -Wall -ansi -pedantic -Werror, what are these flags?**

**-Wall** = show all warnings\
**-ansi -pedantic** = ensures the code is C89 compliant\
\-Werror = treat all warnings as errors!

\-Werror flag is optional and you may avoid using it since you are expected to know the difference between hardline errors and warnings. If you use -Werror flag, you will not see any warnings since all warnings will be treated as errors. _Btw, make sure you fix all your warnings, errors in the code during assignment submissions._

**There are 3 types of errors**

1. Compile-time Errors/Warnings (syntax errors, etc. detectable during compile-time)
2. Runtime Errors (runtime-errors but exceptions)
3. Logical Errors (runtime-errors, but wrong logic)

### Compile-time Errors/Warnings

During compilation you may encounter:

> **Errors:** Remember errors are hardline bugs in your code (i.e. syntax errors) and you will not be able to successfully compile the code with errors. The executable file (output file) will not be created.&#x20;

> **Warnings:** These are soft errors but not bugs. They have the potential to introduce runtime issues (runtime exceptions and logical bugs). Also, fixing the warnings will help you to improve the readability and the maintainability of the code. i.e.
>
> ```c
> int main()
> {
>     int num = 1; 
>     /* WARNING: unused variable */
>     return 0;
> }
>
> /* It is always the best to remove unused variables to improve the
> redability of the code and avoid being into some nasty logical issues 
> in the code */
> ```

### Runtime Errors (Exceptions) - Program Crash

These are the errors you will encounter during the runtime of the program. You will be able to compile the code successfully, but during the runtime of the program due to not handling some specific condition, you may end up with a program crash! (crash due to an exception). These bugs are harder to detect compared to the compile-time errors since the compiler will not help you to spot them. But fixing some warnings may help you to mitigate some of these nasty runtime issues. `gdb` debugger can be used to debug the run time errors most of the time. **We strongly recommend** you to get your hands on `gdb` debugger from week 3 onwards when writing code. It is taught in detail in Week 07, but you may refer to the following link for quick tips.

{% content-ref url="07-testing-and-debugging.md" %}
[07-testing-and-debugging.md](07-testing-and-debugging.md)
{% endcontent-ref %}

Divide by zero is a perfect example of a runtime error. Later, with pointers introduced in week 03, you will encounter a lot of bugs in this category.

{% tabs %}
{% tab title="Runtime Exception" %}
```c
int main()
{
    int num1, num2;
    scanf("%d %d", &num1, &num2);
    printf("Division: %f", num1 / num2);
    return 0;
}

/* This program will successfully compile but will fail in runtime */
```
{% endtab %}

{% tab title="Fix" %}
```c
int main()
{
    int num1, num2;
    scanf("%d %d", &num1, &num2);

    if (num2 != 0)
        printf("Division: %f", num1 / num2);
        
    return 0;
}

/* This program will successfully compile but will fail in runtime */
```
{% endtab %}
{% endtabs %}

### Runtime Errors (Logical Bugs) - Unexpected behaviours

These are the bugs due to your logic being wrong. No compilation errors, no runtime crashes (hopefully), but your program does not provide the expected output. Hence, your program will show unexpected behaviour where you expect something else. These are **worst nightmare grade bugs** and there is no way to detect them unless you review the logic of the code from A-Z. `gdb` debugger can be used to some extent to figure out where the logical bug is, but depending on your experience in coding, it will take few minutes to few hours and few days to troubleshoot the logical bug. The more you code, the more you gain in coding experience so keep it up!

{% tabs %}
{% tab title="Logical Bug" %}
```c
int main()
{
    int num1, num2;
    scanf("%d %d", num1, num2);
    printf("Addition: %d", num1 - num2);
    return 0;
}

/* This program will successfully compile, no runtime-crashes, 
but logically wrong! */

/* Addition does not add two number but taking the difference. 
Therefore, you need to fix the logic of the code. */
```
{% endtab %}

{% tab title="Fix" %}
```c
int main()
{
    int num1, num2;
    scanf("%d %d", num1, num2);
    printf("Addition: %d", num1 + num2); /* FIXED */
    return 0;
}
```
{% endtab %}
{% endtabs %}

&#x20;
