---
description: >-
  Preprocessor directives such as #include, #define and C code compilation
  process, etc.
---

# 02 - Preprocessor, gcc, Makefile

## "Makefile:2: _\*_ missing separator. Stop"

> I keep getting this error when trying to run my makefile. I have used a single TAB on the line after the target file. Changed the \~/.vimrc settings back to standard to see if that would work but still nothing.

**Note:** This issue happened to a student who used VIM text editor. The most likely reasoning is because the .vimrc (VIM configuration file) does not put an exception for a makefile. Please make sure you have the following command at the end of the `.vimrc` file:\
\
autocmd FileType make setlocal noexpandtab\
\
**Explanation:** Makefile needs to use a single TAB space (not 2 or more) for indentation. If the text editor automatically converts any TAB key as multiple space keys, the makefile will not work.\
\
If you use VScode, you can specify that you are writing a makefile when you start a new file. In that case, VScode will automatically use the correct setting for it.

## How do I write a proper make file in UCP?

This is a very resource to understand what's going on with make files

{% embed url="https://opensource.com/article/18/8/what-how-makefile" %}

Following is an example for a basic make file (without variables)

{% tabs %}
{% tab title="Makefile" %}
```bash
#<target> : <dependancies> or <sub-targets>
#		<rules>

# when building the target prog, it looks for its dependancies (main.o, util.o)
# if it finds those targets in the Makefile, it will look for their dependancies.
# if the dependancies have not changed since the last build, nothing will happen.
# if the dependancies have changed since the last build, execute the build rules
# and build the .o files. 
#
# In case, where main.o and util.o files are not found (i.e. initial run), those
# object files will be build according to the targets-rules defined for 
# main.o and util.o defined in the Makefile.
#
prog : main.o util.o
	gcc main.o util.o -o prog

main.o : main.c util.h
	gcc -c main.c -Wall -ansi -pedantic

util.o : util.c util.h
	gcc -c util.c -Wall -ansi -pedantic

clean : 
	rm -f prog *.o # removes all object files *.o all object files

# make clean <- will always run (will never prompt "make: 'clean' up to date")
# since there will be no file called "clean" be made as a result of this operation

#
# How to use Makefile
# -------------------
# make <- will look for the default file named Makefile, build the first target
# make Makefile <- can explicitely define the name of the Makefile
# 
# How to use Makefile to build targets individually
# --------------------------------------------------
# make <target>
# make prog <- build the target "prog"
# make main.o <- build the target "main.o" 
# make util.o <- build the target "util.o"
# make clean <- build the target "clean"

```
{% endtab %}

{% tab title="main.c" %}
```c
#include <stdio.h>
#include "util.h"

int main()
{       
    checkDate();
    printf("Main Function\n");
    return 0;
}
```
{% endtab %}

{% tab title="util.h" %}
```c
#ifndef UTIL_H
#define UTIL_H

void checkDate();

#endif
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>
#include "util.h"

void checkDate()
{
    printf("checkDate() is called\n");
}
```
{% endtab %}
{% endtabs %}

#### In UCP, you **MUST** use one of the following templates to do the make file.

{% tabs %}
{% tab title="Makefile (with variables)" %}
```bash
CC = gcc
CFLAGS = -Wall -pedantic -ansi
OBJ = main.o util.o
EXEC = prog

$(EXEC) : $(OBJ)
	$(CC) $(OBJ) -o $(EXEC)

main.o : main.c util.h
	$(CC) -c main.c $(CFLAGS)

util.o : util.c util.h
	$(CC) -c util.c $(CFLAGS)

clean :
	rm -f $(EXEC) $(OBJ)

# make clean <- will always run (will never prompt "make: 'clean' up to date") 
# since there will be no file called "clean" be made as a result of this operation

#
# How to use Makefile
# -------------------
# make <- will look for the default file named Makefile, build the first target
# make Makefile <- can explicitely define the name of the Makefile
# 
# How to use Makefile to build targets individually
# --------------------------------------------------
# make <target>
# make prog <- build the target "prog"
# make main.o <- build the target "main.o" 
# make util.o <- build the target "util.o"
# make clean <- build the target "clean"
```
{% endtab %}

{% tab title="Makefile (with -D flag)" %}
```bash
# -D defines constants

CC = gcc
CFLAGS = -Wall -pedantic -ansi -D DEBUG=1  # notice the -D flag
OBJ = main.o util.o
EXEC = prog

$(EXEC) : $(OBJ)
	$(CC) $(OBJ) -o $(EXEC)

main.o : main.c util.h
	$(CC) -c main.c $(CFLAGS)

util.o : util.c util.h
	$(CC) -c util.c $(CFLAGS)

clean :
	rm -f $(EXEC) $(OBJ)

# make clean <- will always run (will never prompt "make: 'clean' up to date") 
# since there will be no file called "clean" be made as a result of this operation

#
# How to use Makefile
# -------------------
# make <- will look for the default file named Makefile, build the first target
# make Makefile <- can explicitely define the name of the Makefile
# 
# How to use Makefile to build targets individually
# --------------------------------------------------
# make <target>
# make prog <- build the target "prog"
# make main.o <- build the target "main.o" 
# make util.o <- build the target "util.o"
# make clean <- build the target "clean"
```
{% endtab %}

{% tab title="Makefile (conditional)" %}
```bash
CC = gcc
CFLAGS = -Wall -pedantic -ansi
OBJ = main.o util.o
EXEC = prog

# Add DEBUG to the CFLAGS and recompile the program
ifdef DEBUG_PRINT

CFLAGS += -D DEBUG # appends "-D DEBUG" to the exsting flags defined in CFLAGS

# If rebuild is needed
# Add a new target "DEBUG" to clean "prog" (executable) file. 
# Uncomment the following line
# DEBUG : clean $(EXEC) 
# The removal of $(EXEC) "prog" file will force make tool to rebuild 
# the $(EXEC) "prog" file later in the makefile

endif

$(EXEC) : $(OBJ)
	$(CC) $(OBJ) -o $(EXEC)

main.o : main.c util.h
	$(CC) -c main.c $(CFLAGS)

util.o : util.c util.h
	$(CC) -c util.c $(CFLAGS)

clean :
	rm -f $(EXEC) $(OBJ)


# To run make tool by defining DEBUG_PRINT
# make DEBUG_PRINT=1
#
# ./prog
#
# Output:
# This is debug print 1
# checkDate() is called
# Main Function
# This is debug print 2

# if "DEBUG : clean $(EXEC)" is uncommented above, 
# Every time make DEBUG_PRINT=1 is run, it cleans and builds target $(EXEC) as well 
#
# if you want to build the target without the CFLAG "-D DEBUG"
# make clean /* clean the files from the previous build which includes the DEBUG prints                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       */
# make /* This will build the first target again */
# ./prog
#
# Output:
# checkDate() is called
# Main Function

```
{% endtab %}

{% tab title="main.c" %}
```c
#include <stdio.h>
#include "util.h"

int main()
{
  #ifdef DEBUG
  printf("This is debug print 1\n");
  #endif
  
  checkDate();
    	printf("Main Function\n");
  
  #ifdef DEBUG
  printf("This is debug print 2\n");
  #endif
  
  return 0;
} 

```
{% endtab %}

{% tab title="util.h" %}
```c
#ifndef UTIL_H
#define UTIL_H

void checkDate();

#endif
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>
#include "util.h"

void checkDate()
{
    printf("checkDate() is called\n");
}
```
{% endtab %}
{% endtabs %}

## Is there a quick and easy way to compile all the .c files, generate executable and run the executable?

> I understand that UCP wants us to use makefile to compile a set of .c files. But during coding, I would like to know whether there is a easy and quick way to compile all .c files, generate the executable and run the code at once? Otherwise, I will have to compile each .c file seperate to .o files and then link them together to create the executable and then run it (provided that I still haven't got my makefile done)

&#x20;We always recommend you to make the make file since it efficiently handles the compilation process (it only compile the files which are modified). But lets say, you just want to compile a set of .c files as fast as you can to test some sample codes you have written. Then see below:

{% tabs %}
{% tab title="Comiple/Executable & Run" %}
```c
gcc -Wall -ansi -pedantic *.c -o prog && ./prog
/* In linux, you can combine two commands using && */
```
{% endtab %}

{% tab title="Compile, then executable, then Run" %}
```c
gcc -Wall -ansi -pedantic *.c -c && gcc *.o -o prog && ./prog
```
{% endtab %}
{% endtabs %}

## Do we have bool data type or bool literals in C?

C does not have a boolean literal (`True`, `False`) or boolean type (`bool`). Hence, `0` or NULL is interpreted as False, while any `non-zero` value is interpreted as True.&#x20;

_Note: NULL is a `#define` for `(void*) 0` in the compiler dependant header (`stddef.h`)_

Usually, we `#define` 0 to be FALSE and 1 to be TRUE as below:

```c
#define FALSE 0
#define TRUE 1
/* or */
#define FALSE 0
#define TRUE !FALSE /* !FALSE => !0 => 1 */
```

The reason why we usually **don't** `#define` _any random non-zero value_ to be `TRUE` is because, when negation `!` operator is used on `0`  it returns `1` and vice-versa, thereby maintaining the mathematical property of boolean values `TRUE == !FALSE`

The following examples demonstrate how C interpret boolean values with integer literals:

```c
#include <stdio.h>

/*********************************************************/
// RECOMMENDED WAY IN UCP (TRUE must be 1, FALSE must be 0 or !1 which is 0)
#define TRUE 1
#define FALSE !TRUE
/**********************************************************/

int main()
{
	/* Follwing examples are purely for your knowledge. Refer to the top of this file for the recommended way to define TRUE and FALSE */
	
	/*********************************************************************************************************************************/
	/* NOT TODO: Do not do the following to interpret true and false in C. These examples are purely for the understanding of the concept 
	/********************************************************************************************************************************/

	/* C does not have boolean datatype or boolean literals (True, False) */
	/* Any non-zero used at a place where a condition is expected, will be treated as true in C */
	if (1000)
	{
		printf("Non-zero value in the place of the condition will be interpretted as true\n");
	}
	
	if (0) /* The value 0 or NULL will be interpreted as false */
	{
	
	}
	else
	{
		printf("Zero or NULL in the place of the condition will be interpretted as false\n");
	}


	/* Logical operator ! (not) following a non-zero value, will result in 0 which is then be interpreted as false */
	
	if (!1000)
	{
	}
	else	
	{
		printf("!<any non-zero value> => 0\n");
	}


	/* Logical operator ! (not) following a zero value or NULL, will result in 1 which is then be interpreted as true */
	
	if (!0) /* or (!NULL) */
	{
		printf("!<zero or NULL> => 1\n");
	}
	else	
	{
		
	}

	
	/* Checkout the following printf to verify the concepts above */

	printf("Boolean TRUE, FALSE: %d, %d\n", TRUE, FALSE);
	printf("Interpreted true: %d, %d, %d\n", 10, (!(!10)), !NULL);
	printf("Interpreted false: %d, %ld\n", !10, (long int)NULL);
		
	return 0;
}

```

## Can I pass an integer to a function that expects a char type parameter or vice-versa?

Yes, you can. But remember char has **only** 8 bits (platform dependant). Therefore you can only store numbers from (-128) to 127 (-2^7 to (2^7) - 1). The signed data types such as (char, int) utilize the 1st bit to indicate the sign of the number. You will learn the representation of signed variables and unsigned variables and the effect of bit-shifting on them in "Misc C" Lecture.

```c
char ch;
ch = 'A'; /* All good. 'A' is a char literal */
ch = 65; /* All good. 65 is a integer literal */
ch = "A"; /* ERROR: "A" is a string literal */
```

Furthermore, since computers cannot understand characters 'A', 'B', etc, but numbers (in binary) every character on the keyboard corresponds to a special code called "ASCII code"  (or Unicode - the extended version of ASCII). Therefore if you assign a char literal to an integer variable, it means you are assigning the corresponding ASCII value of it. Please refer to the ASCII code table[ here.](https://www.ascii-code.com/)

```c
int num = 'A'; /* num will be 65 */
char ch = 'A'; /* ch will be 65 */

printf("%d", num); /* prints 65 */
printf("%d", ch); /* prints 65 */
printf("%c", num); /* prints 'A' */
printf("%c", ch); /* prints 'A' */


/* 
char simply means an allocation of 8 bits (platform dependant)
whereas 
int simply means an allocation of 32 bits (platform dependant)
*/
```

Basically, invoking functions with char type parameters by passing an integer literal or vice versa follow the same concepts explained above.

```c
#include <stdio.h>

void fCh(char ch)
{
    printf("%d", ch); /* prints the numerical value of ch */
    printf("%c", ch); /* prints the character corresponds to the value
    of ch using ASCII conversion table */
}

void fInt(int num)
{
    printf("%d", num); /* prints the numerical value of num */
    printf("%c", num); /* prints the character corresponds to the value
    of num using ASCII conversion table */
}

int main()
{
    fCh('A');
    fCh(65);
    
    fInt('A');    
    fInt(65);
    
    return 0;
}
```

## What's the purpose of gcc -D flag?

> What is the purpose of gcc with -D flag to define constants when we have the ability to directly `#define` them in the .c file itself?

`-D` flag defines constant(s) at the time of compilation in contrast to those constants defined using `#define` in the source code (.c or .h file) itself.

```c
gcc -Wall -ansi -pedantic main.c -D DEBUG=1 -o prog
/* defines a constant name DEBUG with the value 1. 
Make sure to indicate the value of the constant with = sign to
successfully run the gcc statement. */
```

Imagine you are working with a large codebase in a company. You as the developer, have put a lot of debug logs in the code as shown below to troubleshoot any issues encountered in the production. Once the coding is completed in the development stage, your source codes will be moved to system support engineers who are responsible to build the code (compile and create executables/libraries which are optimized for the deployment on the production environment of the client) and pack them, deploy them on the production environment.

```c
double add(int num1, int num2)
{
    #ifdef DEBUG
    printf("[add] debug print, accepts: %d %d \n", num1, num2);
    #endif
    
    int ans = 0;
    ...
    ...
    ...
    
    #ifdef DEBUG
    printf("[add] debug print, returns: %d \n", ans);
    #endif
    
    return ans;
}
```

Remember, `printf(...)` is a costly function call in terms of the performance of your program due to the involvement in I/O operations. Therefore, the system support guys need a way to get rid of your debug logs (overhead) without changing your source files. In this case, they can simply use `gcc` without the `-D` flag constant`DEBUG=1`to compile and build executables/libraries out of your code.

in UCP, we have only focused on building executables. But`gcc`can be used to create distributable libraries (refer to this [link](https://curtin-ucp.gitbook.io/faq/02-environments#another-good-reason-to-use-header-files-is)) as well.

## What is the difference between Macro and Function?&#x20;

* A macro is a fragment of code that has been given a name.
* Whenever the name is used, it is replaced by the contents of the macro.
* Macros resemble function calls **(but they are not functions).**
* Works by substitution.

| Macro                                                                  | Functions                                                                                                                                                     |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Macro calls are replaced with macro expressions.                       | In a function call, the control is passed to a function definition along with arguments, and a definition is processed and the value may be returned to call. |
| Macros run programs faster but increase the program size.              | Functions run programs slower but decrease the program size and compact.                                                                                      |
| It is better to use Macros, when the definition is very small in size. | It is better to use functions when the definition is bigger in size.                                                                                          |

## Turning multiline function into a Macro

> How to convert a function into a macro?

Try summarizing the function into a boolean statement which can then be set to a Macro.

{% tabs %}
{% tab title="Function" %}
```c
#include <stdio.h>

#define TRUE 1
#define FALSE 0 /* or !TRUE */

int test(int i)
{
    int iRetValue = TRUE;
    
    if (i > 10)
    {
        iRetValue = FALSE;
    }
        
    return iRetValue; 
}

/* above function is equvelant to: */

int test(int i) 
{
    return (!(i > 10)); /* single line boolean expression */
}

/* Therefore, we can convert it to a MACRO easily */

```
{% endtab %}

{% tab title="TEST_MACRO" %}
```c
#define TEST_MACRO(I) (!(I > 10))

/* or */

#define TEST_MACRO2(I) (I <= 10)

/* IMPORTANT: do not use semi-colons when writing macros
since the whole expression get replaced in MACRO expansion
process which is performed by the preprocessor during 
preprocessing stage */

/* Macro Expansion: Preprocessor replaces Macros just like 
'find and replace' functionality in a typical word processor
application */
```
{% endtab %}
{% endtabs %}

The following code shows how to convert a no-return function to a macro. This is **NOT\_RECOMMENDED** in UCP since you will **NOT be asked** to convert a no-return function to a macro.

{% tabs %}
{% tab title="Function" %}
```c
#include <stdio.h>

void noReturn(int i)
{
    if (i > 10)
    {
        printf("Number is larger than 10\n");
    }
    
    return; /* or return void; or avoid the whole return statement */
}
```
{% endtab %}

{% tab title="NORETURN_MACRO" %}
```c
#include <stdio.h>

/* 
NOT_RECOMMENDED: In UCP, avoid defining macro as below. 
This is just FYI. Note that macros are just 
find and replace statements
*/
#define NORETURN_MACRO(I) if (I > 10) {printf("Number is larger than 10\n");}

int main() 
{
    NORETURN_MACRO(11)
    return 0;
}

```
{% endtab %}
{% endtabs %}

## Can I use MACRO to replace anything?

Yes, you can since macro simply works as find and replace statements. Expanding a macro means, find the occurrences of a macro in the code, replace it with the statement defined for the macro replacing the variables if there are any.

In UCP, it is **NOT RECOMMENDED** to use macros as you wish! violating the coding standards and affecting the readability of the code. Please **use** macro **ONLY** for those [short functions with a return value](https://curtin-ucp.gitbook.io/faq/02-environments#turning-multiline-function-into-a-macro).

A bad example:

```c
#include <stdio.h>

/* ************************************************************************************** */
/* NOT TODO: Never use #define in the following way in UCP! */
/* These examples are purely for your understanding of the concept */

#define CHECK if /* <- this is the syntax if */

/* following macro expands to a block of code! */
#define CHECK_GREATER_100(X) if (X > 100) { printf(">100\n"); } else { printf("<=100\n"); }
/* ************************************************************************************** */

int main()
{	
	int num = 150;
	CHECK_GREATER_100(50)
	
	CHECK (num > 200)
	{
		printf(">200\n");
	}
	else {
		printf("<=200\n");
	}
		
	return 0;
}
```

## What are static functions and variables?

`static` functions and `static` variables are not in any way related to one another!

### Static Functions

* `static` syntax is a storage class specifier. Not a mandatory part for function header/signature.
* `static` will make the function **private to the .c file where it is defined** (function definition not declaration)

{% tabs %}
{% tab title="main.c" %}
```c
#include <stdio.h>

void checkDate();

/* You can declare this function (prototype) in any 
other .c file and use it without any issue. */
void nonstatic_testMain()
{
	printf("nonstatic_testMain\n");
}

/* This function is private to main.c file. Cannot be 
declared (prototype) and used in any other .c file. 
An error will be reported by the linker if you do so 
because the linker cannot locate the definition in 
main.c file. Remember it is hidden! therefore util.c 
cannot see it! */
static void static_testMain()
{
	printf("nonstatic_testMain\n");
}

/* static function declaration (prototype) can be moved 
to a seperate header file. But still, static will still 
make the function private to where it is defined */

int main()
{
	checkDate();
	return 0;
}

/* compile the code with
gcc -Wall -ansi -pedantic main.c util.c -o prog */
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>

void nonstatic_testMain();

void static_testMain(); 

void checkDate()
{
	nonstatic_testMain();
	static_testMain(); /* ERROR: Cannot use private function 
	of main.c. Linker will complain */
	printf("checkDate()\n");
}
```
{% endtab %}
{% endtabs %}

### Static Variables

* `static` makes a local variable last until the end of the program! (not the function-end where it is defined) - belongs to the global space of the memory
* Ordinary local variables disappear when a function ends.
* `static` local variables don’t disappear.
* They keep their values between function calls.
* Initialised only once, when the function is first called.

```c
#include <stdio.h>

int add()
{
	/* This statement will only be execute once and once only 
	when the function add() is invoked for the first time. 
	The static local variable num will stay alive until the 
	end of the program (not the function-end). */
	static int num = 0; 

	num = num + 1;
	printf("Result: %d\n", num); /* will print 1, 2, 3 if add() 
	is invoked thrice */

	return num;
}

int add2()
{
	static int num;
	num = 0;

	num = num + 1;
	printf("Result: %d\n", num); /* will print 1, 1, 1 if add() 
	is invoked thrice */

	return num;
}

int main()
{
	checkDate();
	add();
	add();
	add();
	add2();
	add2();
	add2();
	return 0;
}
```

## Static Functions in Headers?

> When I add static functions inside .h file, it gives me errors when trying to test it in main?

Declare static function / prototype on top of the .c file. Because the static function is not supposed to be known to other files (static function is private to the .c file where it is defined), there is no need to put it in the header file. Remember, static function definitions on a .c file are hidden to other files.

## GCC Compiler in Detail?

You may find the following flags for `gcc` are useful in understanding what's going on under the hood during the compilation process.

```
gcc -E main.c       // stops after preprecessing (console output)
gcc -S main.c       // stops after initial compilation (Assembly file)
gcc -c main.c       // stops after "compiling" but no linking (object file test.o)
gcc main.c          // stops after linking (executable file a.out or a.exe)
gcc main.c -o prog  // stops after linking (executable file is prog)

gcc -Wall -ansi -pedantic util.c main.c -o prog // stops after linking. Compilation happens with the flags (executable file is prog) - Recommended in UCP Unit (Compiling + Linking)
gcc -Wall -ansi -pedantic util.c main.c -o prog -D DEBUG=1 // Preprocessor name DEBUG can be defined on the compiler command-line as well
gcc -g - Wall -ansi -pedantic util.c main.c -o prog // Compile the information required for valgrind detailed error messages
```

### GCC Compiler (Linker)

```
gcc -Wall -ansi -pedantic -c main.c // Recommended in UCP Unit (Compile only)
gcc -Wall -ansi -pedantic -c util.c
gcc main.o util.o -o prog // Recommended in UCP Unit (For Linking)
```

* **Linker** will link all object files to generate the executable file.
* During this process, **Linker** will look for the function definitions (prototype + body) in all object files.
* Linker errors are not descriptive as compiler errors since **linker works only with the object files** and line no, column no information can not be provided in linker errors.
* In contrast, **compilation** errors refer to the errors before linking happens. They are descriptive (line number, column no, file name, etc can be provided) since it **works with the .c files.**

### &#x20;**An in-depth look at GCC Compiler**

Let us have a closer look at what happens with the following  compilation statement

```
gcc -Wall -ansi -pedantic util.c main.c -o prog
```

During a typical generation of an executable file, the following takes place.

1. C Code -> Preprocessed C Code // Preprocessor does this
2. Preprocessed C Code -> Assembly Code (not machine code, but hardware dependant barely human-readable code) // Compiler does this
3. Assembly Code -> Object File (machine code) // Assembler does this
4. Object File -> Executable File (machine code) // Linker

Step 1, 2, and 3 altogether are known as **"Compiling"** (`gcc` with `-c` flag)\
Step 4 is known as **"Linking"** (`gcc` with `-o` flag)

_**Special notes:** Preprocessor, Compiler, Assembler, and Linker are all parts of GCC._

#### **Detail Explanation of the Steps above:**&#x20;

**In Step No1:** \
The preprocessor will resolve all preprocessor directives (#include, #define, #ifdef - #endif, #ifndef - #endif, etc.)

* `gcc` **-E** `main.c` // will output the preprocessed C code to the console. Feel free to have a look and see what a preprocessed C code looks like.

**In Step No2:**\
The compiler will see the preprocessed C code and compiles it to the assembly code.

* Preprocessed C code will not have any preprocessor directives (`#include`, `#define`, `#ifdef` - `#endif`, `#ifndef` - `#endif`, statements proceeded by #) since they are already processed at step No 1.
* The initial compilation that happens at step No. 2 is **completely unaware of any preprocessor directives. In fact, it will fail if it sees any.**
* `gcc` **-S** `main.c` // will output the assembly code. Feel free to have a look at the assembly code which is barely human-readable.

**In Step No3:** \
The assembler will generate the object file for assembly code.

* `gcc` **-c** `main.c` // will output the object file

**In Step No4:** \
The linker will link all object files to generate the executable file.

* `gcc main.o` **-o** `prog` // will output the executable file

## What are Compiler and Linker Errors?

These are the errors that could occur during compilation. But not at runtime. Errors could be categorized as follows:

1. Compile-time Errors/Warnings (includes Linker Errors) - happens at compile time
2. Runtime Errors (runtime-errors but exceptions)
3. Logical Errors (runtime-errors, but wrong logic)

Please refer to: for mode details.

[01 C Basics | Error Types](https://curtin-ucp.gitbook.io/faq/01-c-basics#gcc-wall-ansi-pedantic-werror-what-are-these-flags)

It is important to distinguish compiler errors/warnings (syntax errors) from linker errors. Compiler errors (syntax errors) will be identified during the compilation stage ([step no2 in gcc compiler](https://curtin-ucp.gitbook.io/faq/02-environments#an-in-depth-look-at-gcc-compiler)), while linker errors will be identified in [step no4 linking stage](https://curtin-ucp.gitbook.io/faq/02-environments#an-in-depth-look-at-gcc-compiler).

**Compilation Errors:**

gcc can provide extra details on compilation errors such as filename, line no, column no as well as some description about the error. This is because the compilation process ([step 02 in gcc compiler](https://curtin-ucp.gitbook.io/faq/02-environments#an-in-depth-look-at-gcc-compiler)) takes the **.c files** as the input. **compilation** errors refer to the errors before linking happens.

During compilation, the function definitions will not be looked into by the compiler. The compilation process is happy to go ahead with just the function declarations. This is the reason why the following code could be compiled successfully without any errors popping up for not being able to find function definitions.

```c
 
```

**Linker Errors**&#x20;

gcc  can provide less details on linker errors since the linking stage (step04 in gcc compiler) takes the .o files (machine code) as the input. This is where you see weird looking errors.

Remember linker is responsible to combine **.o files** together to produce the executable file and more importantly find the **function definitions** across all .o files. If not found, it will report an error.

## **Header File (.h) Usage**

Following is recommended for using header files in your code.

* List all function declarations (prototypes) but not the definitions.
* List the #define constants and macros
* List the typedefs
* Use header guards to avoid multiple includes
* Remember, the .h file lists the public API (function decl**a**rations, etc) which you hope to use in other .c files&#x20;
* Never state the .h files in your compiler command gcc.
* **use gcc to compile .c files only.** (the compiler knows which .h files are needed and how to locate them when preprocessing #include directive)&#x20;

### Can I compile and link source codes without using header files?

Yes, but it is **NOT RECOMMENDED** in UCP. As long as you forward declare the function in .c file where it is used, you will be able to successfully compile and link. Use the .o (corresponds to .c) file where the function is defined during the linking stage to let the linker find the definition for the forward declared function.&#x20;

&#x20;For Example:

{% tabs %}
{% tab title="main.c" %}
```c
#include <stdio.h>

void checkDate();

int main()
{		
    checkDate();
    return 0;
}

/*

// Following will work and main.o will be created successfully 
since the compilation only needs a function to be forward declared 
(prototype). Note that im only compiling the main.c file 
with -c flag. Im not calling the linker to generate the 
executable file.

gcc -Wall -ansi -pedantic -c main.c


// linked will fail since the linker cannot find the function 
definition for checkDate() function in main.c file used 
in the gcc command below:

gcc -Wall -ansi -pedantic main.c -o prog1


// following will successfully compile because the linker 
can locate the function definition (protoype + body) for 
the function checkDate() in util.c

gcc -Wall -ansi -pedantic main.c util.c -o prog2

*/
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>

void checkDate()
{
    printf("checkDate()\n");
}
```
{% endtab %}
{% endtabs %}

### How do I compile using the header file?

{% tabs %}
{% tab title="main.c" %}
```c
#include "util.h" /* relative path from the current directory */

int main()
{		
    checkDate();
    return 0;
}

/*
// Observe the preprocessed C output by using the flag -E
// See what #include does. It simply copies the content in Util.h to main.c file.
gcc -Wall -ansi -pedantic -E main.c

gcc -Wall -ansi -pedantic main.c -o prog1
gcc -Wall -ansi -pedantic main.c util.c -o prog2

gcc -Wall -ansi -pedantic -c main.c util.c

Absoute path for #include with "" marks, compile it with -I flag
gcc -Wall -ansi -pedantic -I <directory> ...
*/

```
{% endtab %}

{% tab title="util.h" %}
```c
void checkDate();
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>
/*#include "util.h"*/ /*this include is not necessary unless you have typedef in the header file*/

void checkDate()
{
    printf("checkDate()\n");
}
```
{% endtab %}
{% endtabs %}

#### Wait what, where is a header guard in util.h? Do we really need header guards?

Yes, we do; when multiple headers are included in a .c file. This is to avoid multiple includes of the same header file and the issues that arise from it.&#x20;

For Example:

{% tabs %}
{% tab title="main.c" %}
```c
#include <stdio.h>
#include "util.h" /* includes the content from util.h */
#include "util2.h" /* includes the content from util.h 
via util2.h and the content specific 
to util2.h - MULTIPLE INCLUSION OF util.h header file */

/* To avoid multiple inclusion of the header file util.h, use header guards */

int main()
{
    checkDate();
    return 0;
}
```
{% endtab %}

{% tab title="util.h" %}
```c
#ifndef UTIL_H 
#define UTIL_H
/* Comment the header guard and observe 
the pre-processed source via gcc -E flag */

void checkDate();

#define PI 3.14

#endif
```
{% endtab %}

{% tab title="util.c" %}
```c
#include <stdio.h>

void checkDate()
{
    printf("checkDate()\n");
}
```
{% endtab %}

{% tab title="util2.h" %}
```c
#ifndef UTIL2_H
#define UTIL2_H

#include "util.h"
#define LENGTH 10

#endif
```
{% endtab %}
{% endtabs %}

Btw, multiple declarations of the same function (on a .c file itself or via `#include` multiple headers) or multiple `#define` for the same constant name and **value** (on a .c file itself or via `#include` multiple headers) will not cause any issues. But multiple `typedef` for the same type definition (on a .c file itself or via `#include` multiple headers) will cause issues.

For Example:

{% tabs %}
{% tab title="Example 01" %}
```c
#include <stdio.h>

#define PI 3.14
#define PI 3.14 /* No warning since it is the same value for the #define constant PI */
/*#define PI 3.141*/ /* WARNING: "PI" redefined */

int main()
{
    printf("%f", PI);
}
```
{% endtab %}

{% tab title="Example 02" %}
```c
void testMain(); /* declaration of testMain() */
void testMain(); /* No warning since it is the redeclaration 
of the function but not the definition */

int main()
{	
    return 0;
}


void testMain() /* definition of testMain() */
{
}

/* If we try to redefine testMain() below */
/* ERROR: redefintion of testMain() */
/*
void testMain() 
{
}
*/
```
{% endtab %}

{% tab title="Example 03" %}
```c
/* typedef is a statement that defines a new type. This is
particularly helpful in defining types for complex 
C structs where multiple primitive type fields 
(char, int, float, double, etc) exist in.
Refer to C struct for more details. */


/* Remember typedef(s) are not #define(s)
typedef are regular C statements and pre-processor will NOT
process typdefs */

/* the following typedef defines a new type called myInt 
from an existing primitive type int */

typedef int myInt; 
/*typedef int myInt;*/ /* redefinition of typedef ‘myInt’ */
/*typedef double myInt;*/ /* conflicting types for ‘myInt’ */

int main()
{
    return 0;
}
```
{% endtab %}
{% endtabs %}

Therefore to keep things nice and simple, in UCP, it is **RECOMMENDED** to **always use header guards** in header files.

## Do we really need header files?

They are particularly useful on quite a few occasions. Assume that you are working on a big project with 3 developers.&#x20;

**Developer 01:** writes `maths.c` that includes all the math-related functions

**Developer 02:** writes `graphs.c` that includes all the graph-related functions

**Developer 03:** uses functions from math library (dev01) and the graph library (dev02)

### Lets look at how they can work together.

* Developer 03 asks Developer 01 to write implementations for `sin(), cos(), tan(), add(), subtract(), divide(), multiply(), shiftBits()` functions
* Developer 03 asks Developer 02 to write implementations for `chart(), lineChart(), pleChart(), barChart(), drawTrangle(), drawSqaure()`functions
* Developer 03 wishes to use the functions above in multiple .c files (main.c, util.c, environment.c)

Lets take a look at _Developer 01 codebase:_

{% tabs %}
{% tab title="maths.h" %}
```c
#ifndef MATHS_H
#define MATHS_H

/* Following is the public interface for maths library */
int sin();
int cos();
int tan();
int add();
int subtract();
int divide();
int multipy();
int shiftBits();

#endif
```
{% endtab %}

{% tab title="maths.c" %}
```c
#include "math.h"

int pvtSinCalc();
int strictlyPvtCosCalc();

int sin()
{
    /* Assume pvtSinCal() is an internal function which is invoked to
    calculate the sin() value */
    pvtSinCal();
    ...
}

int cos()
{
    /* Assume strictlyPvtCosCalc() is an internal function which is invoked to
    calculate the cos() value */
    strictlyPvtCosCalc();
    ...
}

int tan()
{
    ...
}

int add()
{
    ...
}

int subtract()
{
    ...
}

int divide()
{
    ...
}

int multipy()
{
    ...
}

int shiftBits()
{
    ...
}

/* the following function could be forward declared in any other
.c file and can be used eventhough it is not exposed via math.h */
int pvtSinCalc() 
{
    ...
}

/* same as above, but if forward declared in some other .c file, 
you will not be able to use it since the linker is unable to see
this function definition from other .c files (static functions
are private to where they are defined). */
static int strictlyPvtCosCalc() 
{
    ...
}
```
{% endtab %}
{% endtabs %}

_Developer 02 codebase:_

{% tabs %}
{% tab title="graphs.h" %}
```c
#ifndef GRAPHS_H
#define GRAPHS_H

/* Following is the public interface for graphs library */
void chart();
void lineChart();
void pleChart();
void barChart(); 
void drawTrangle()
void drawSqaure();

#endif
```
{% endtab %}

{% tab title="graphs.c" %}
```c
#include "graphs.h"
#include "maths.h" /* Developer 02 may need functions from maths.h library */

void chart() 
{
    add() /* from maths.h */
    ...
}

void lineChart() 
{
    ...
}

void pleChart() 
{
    ...
}

void barChart()
{
    ...
}

void drawTrangle()
{
    ...
}

void drawSqaure()
{
    ...
}
```
{% endtab %}
{% endtabs %}

_Developer 03 codebase:_

{% tabs %}
{% tab title="main.c" %}
```c
/* if main.c uses functions from graphs or maths library, 
it needs to forward declare all the functions of graphs.c 
and maths.c in this file before invoking them. Refer below: */

int sin();
int cos();
int tan();
int add();
int subtract();
int divide();
int multipy();
int shiftBits();

void chart();
void lineChart();
void pleChart();
void barChart(); 
void drawTrangle()
void drawSqaure();

int main()
{
    chart();
    ...
    return 0;
}

/* you may need to use other forward declared functions later on 
when you decide to extend main.c */
```
{% endtab %}

{% tab title="environment.c" %}
```c
/* if environments.c uses functions from graphs or maths library, 
it needs to forward declare all the functions of graphs.c 
and maths.c in this file before invoking them. Refer below: */

int sin();
int cos();
int tan();
int add();
int subtract();
int divide();
int multipy();
int shiftBits();

void chart();
void lineChart();
void pleChart();
void barChart(); 
void drawTrangle()
void drawSqaure();

void createEnv() /* this function is defined in environments.c */
{
    drawSquare();
    ...
}

/* you may need to use other forward declared functions later on 
when you decide to extend environments.c with many funtions */
```
{% endtab %}

{% tab title="util.c" %}
```c
/* if util.c uses functions from graphs or maths library, 
it needs to forward declare all the functions of graphs.c 
and maths.c in this file before invoking them. Refer below: */

int sin();
int cos();
int tan();
int add();
int subtract();
int divide();
int multipy();
int shiftBits();

void chart();
void lineChart();
void pleChart();
void barChart(); 
void drawTrangle()
void drawSqaure();

void showTrends() /* this function is defined in util.c */
{
    lineChart();
    ...
}

/* you may need to use other forward declared functions later on 
when you decide to extend util.c with many funtions */
```
{% endtab %}
{% endtabs %}

_Developer 03 codebase_ **refactored into header files:**

{% tabs %}
{% tab title="main.c" %}
```c
#include "maths.h"
#include "graphs.h"
/* This will include the entire public API 
(Application Programming Interface) - all exposed function 
declarations via maths.h and graphs.h for maths library and 
graphs library respectively. */

int main()
{
    chart();
    ...
    return 0;
}
```
{% endtab %}

{% tab title="environment.c" %}
```c
#include "maths.h"
#include "graphs.h"
/* This will include the entire public API 
(Application Programming Interface) - all exposed function 
declarations via maths.h and graphs.h for maths library and 
graphs library respectively. */

void createEnv() /* this function is defined in environments.c */
{
    drawSquare();
    ...
}
```
{% endtab %}

{% tab title="util.c" %}
```c
#include "maths.h"
#include "graphs.h"
/* This will include the entire public API 
(Application Programming Interface) - all exposed function 
declarations via maths.h and graphs.h for maths library and 
graphs library respectively. */

void initProgram() /* this function is defined in init.c */
{
    lineChart();
    ...
}
```
{% endtab %}
{% endtabs %}

Using header files, it will be easier to abstract out a public API for the other developers who rely on your library code. They can also start working in parallel with you, provided that the public API is fixed in the initial stages of development. The public API decouples the main.c, environments.c and util.c from the actual implementations of maths and graphs (maths.c, graphs.c) library.

Later on, if Developer 01 adds more functions to the public API of maths library (maths.h), Developer 02, and 03 can easily use them in their code without needing to forward declare the functions in each .c file (main.c, environment.c, util.c) prior to use.

### **Another good reason to use header files is:**

Assume you are a 3rd party library developer. You have developed a highly complex machine learning library and you would only like to share the public API of the library (.h file) with other developers but not the actual implementation of the functions (.c file).

In this case, you can compile the .c file to a .o file (machine code) using `gcc -c` and simply provide the .o file along with the .h file as the public API to other developers.

Furthermore, if we have multiple .o files which need to be shared among other developers, we can pack a set of .o files into static library or dynamically loaded library and pass it across. This is out of the scope of UCP. Please read below for more details

{% embed url="https://cs-fundamentals.com/c-programming/static-and-dynamic-linking-in-c" %}



