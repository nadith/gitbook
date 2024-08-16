---
description: arrays, sizeof, strings, string methods
---

# 04 - Arrays & Strings

## const Pointer vs pointer to const, what is the difference?

* const pointer: **pointer value** is constant and cannot be changed after initialization.
* pointer to const: **pointed value** is constant

{% tabs %}
{% tab title="main" %}
```c
#include <stdio.h>

int main()
{
	/* 
	Const Pointer Types
	--------------------
	- Pointer value constant: int* const piPtr;	// value of the pointer cannot be modified 
	- Pointed value constant: const int* piPtr; 	// pointed value cannot be modified 
	*/

	int iNum = 10, iNum2 = 20;

	/* Pointer Value Constants */
	int* const piPtr;		/* piPtr will be set with a garbage value */
	/* piPtr = &iNum;*/ 		/* ERROR: initialization is not at declaration time */
	int* const piPtr2 = &iNum; 	/* IMPORTANT: Initialization can only happen at declaration */

	/* Pointed Value Constants */
	int const *piPtr3; 		/* or const int* piPtr;*/

	/* Pointer and Pointed Value Constants */
	int const * const piPtr4 = &iNum; /* or const int* const piPtr;*/

	/* Arrays */
	int arrNum3[3]; 			
	int arrNum3_2[3] = {10, 20, 30};	

	/* Pointer Value Constant Examples 
	   ------------------------------- */
	*piPtr2 = 100; 			/* can modify the pointed value */
	/*piPtr2 = &iNum2;*/ 		/* ERROR: cannot modify pointer value */
	
	/* Pointed Value Constant Examples
   	 ------------------------------- */
	piPtr3 = &iNum;
	piPtr3 = &iNum2; 		/* can be assigned multiple times */
	/* *piPtr3 = 200; */ 		/* ERROR: cannot modify pointed value */
	
	
	/* Pointer and Pointed Value Constant Examples
	   ------------------------------------------- */
	/*piPtr4 = &iNum2;*/ 		/* ERROR: cannot modify pointer value */
	/* *piPtr4 = 200; */ 		/* ERROR: cannot modify pointed value */

	/* Array is a Pointer Value Constant
	   --------------------------------- */
	/*arrNum3 = arrNum3_2;*/ 	/* ERROR: arrNum3 is a int* const pointer. The value of the pointer cannot be changed */

	printf("piPtr:%p, piPtr2:%p, piPtr3:%p, piPtr4:%p\n", (void*)piPtr, (void*)piPtr2, (void*)piPtr3, (void*)piPtr4);
	printf("Arrays: %p %p\n", (void*)arrNum3, (void*)arrNum3_2);

	/* Double Pointer Example: Refer to the function above */
	doublePointerExamples();

	return 0;
}
```
{% endtab %}

{% tab title="doublePointerExamples" %}
```c
#include <stdio.h>


void doublePointerExamples()
{
	/* 
	-----------------------
	Double Pointer Examples 
	-----------------------*/
	
	int* piPtr, iNum = 10;	

	/* ppiPtr is a pointer value constant */
	int** const ppiPtr = &piPtr; 	/* IMPORTANT: Initialization can only happen at declaration */

	/* ppiPtr2 is a pointed value constant (1st level) */
	int* const *ppiPtr2; 

	/* ppiPtr2 is a pointed value constant (2nd level) */
	int const **ppiPtr3;	 	/* or const int** piPtr */
		

	ppiPtr2 = &piPtr;
	/* *ppiPtr2 = &iNum; */		/* ERROR: assignment of read-only location ‘*ppiPtr2’ */
	piPtr = &iNum;			/* But can assign through non-const pointer */						
	
	*ppiPtr3 = &iNum;
	**ppiPtr3 = 150;		/* ERROR: assignment of read-only location ‘**ppiPtr3’ */
		

	/* const casting */
	ppiPtr2 = (int* const *)&piPtr;
	ppiPtr3 = (int const **)&piPtr;

	printf("ppiPtr:%p, ppiPtr2:%p, ppiPtr3:%p\n", (void*)ppiPtr, (void*)ppiPtr2, (void*)ppiPtr3);
}	

```
{% endtab %}
{% endtabs %}

## What are arrays?

* Arrays in C are **accessed one element at a time**.
* Arrays could be allocated memory **statically** or **dynamically**. (do not confuse with `static` keyword)
* Statically declared arrays are allocated memory at compile-time and array length must be compile-time constant (C89 requirement)!
* Dynamically declared arrays using malloc are allocated memory at run time.
* **Array variable** (static array) can be seen as a **const pointer**, but not a pointer to const
* The name of the _array_ is a pointer to the first element.
* _array_\[0] == \*_array_
* &_array_\[0] == _array_
* _array\[1] == \*(array + 1)_

```c
/* Statically declared array */
int arriNums[10]; /* Length: must be a compile time constant */

/* Dynamically declared array */
int len = 10;
int* piNums = (int*) malloc (size(int) * len);

arriNames = piNums; /* ERROR: arriNames could be seen as a const pointer. 
Hence the value of the pointer cannot be changed. */
```

### &#x20;**Array Initialization using '{' '}' notation**

* **Statically declared arrays** can be initialized using '{' '}' notation at the time of declaration.
* When declaring arrays and initializing with '{' '}':
  * **The size of the first dimension is optional.**
  * **Multi-dimensional** arrays: **Except for the first dimension**, the **correct sizes for other dimensions** must be specified.

```c
#include <stdio.h>

int main()
{
	int arrNumGarbage[3]; /* Elements will be initialized to garbage values */

	/* Array initialization via  {..., ..., ...} 
	- Can only be used when initializing an array at the declaration. 
	- Declaration and initialization should happen on the same line.
	*/
	int arrNum3[] = {10, 20, 30};
	int arrNum1[1] = {10}; /* Array with single element, similar to integer variable */
	int arrNum2[2] = {10}; /* 2nd element set to zero */
	int arrNum22[2] = {10, 20, 30}; /* Compiler warning */

	/* {..., ..., ...} cannot be used after array declaration */
	/*arrNum3 = {100, 200};*/ /* ERROR */
	/*arrNum2 = {100, 200};*/ /* ERROR */

	printf("%d %d\n", arrNum[0], *arrNum);
	printf("%d %d\n", arrNum1[0], *arrNum1);
	printf("%d %d %d\n", arrNum2[0], *arrNum2, arrNum2[1]);
	printf("%d %d\n", arrNum22[0], *arrNum22);

	return 0;
}
```

#### **{ } notation cannot be used to initialize pointers but static (not dynamic) arrays.**

```c
int main() 
{
    int arrNums[] = {0, 0, 0};
    /* Note that arrNums[] is a static array declaraion.
     * Therefore { } notation can be used to initialize 
     * the elements */
    
    int* piNums = {0, 0, 0}; /* Error */
    /* Note that piNums is just a pointer but not an array.
     * Therefore { } notation can NOT be used. */
    
    return 0;
}

```

> Althrough an array could be seen as a const pointer **after the declaration (not during declaration)**, they are not strictly a pointer type but array type. To keep things simple, you can assume it works as a pointer type after the declaraion in most cases. For more details refer to the section below on sizeof ([link](https://curtin-ucp.gitbook.io/faq/04-arrays-and-strings#how-does-sizeof-work-with-arrays)) - refer to size of (advanced) tab.

### **Array Parameters in a Function**

* The **size of the first dimension is optional**. _The compiler ignores it even if the correct or incorrect size is specified._
* Passing **multi-dimensional** arrays: **Except for the first dimension**, the **correct sizes for other dimensions** must be specified.&#x20;
* if not, WARNING: expected ‘int (\*)\[10]’ but argument is of type ‘int (\*)\[4]’

```c
void func1(char arrcCh[10])
{
	printf("sizeof(arrcCh): %ld\n", sizeof(arrcCh)); /* WARNING: ‘sizeof’ on array function parameter ‘arrcCh’ will return size of ‘char *’ */
	printf("sizeof(*arrcCh): %ld\n", sizeof(*arrcCh));	/* 1 */
}

void func2(int arri2dNum[][10])
{
	printf("sizeof(arri2dNum): %ld\n", sizeof(arri2dNum)); /* WARNING: ‘sizeof’ on array function parameter ‘arri2dNum’ will return size of ‘int (*)[10]’ */
	printf("sizeof(*arri2dNum): %ld\n", sizeof(*arri2dNum)); /* 40 */
}

void func3(char* piNum)
{
}

int main()
{
	char arrcCh[4]; 			/* statically allocated - stack*/
	int arri2Num[3][4]; 	/* statically allocated - stack */
	char* piNums = (char*) malloc(sizeof(char) * 4); /* dynamically allocated - heap */

	func1(arrcCh); 
	func2(arri2Num); /* WARNING: expected ‘int (*)[10]’ but argument is of type ‘int (*)[4]’ */
	func3(piNums);

	return 0;
}
```

### How does sizeof(...) work with arrays?

* It is a compile-time unary operator and **used to compute the size of its operand**
* brackets are optional
* returns no. of bytes
* **works at compile time**

{% tabs %}
{% tab title="sizeof basics" %}
```c
void func(char arrcCh[], int arri2dNum[][4], char* piNum)
{
	sizeof(arrcCh); 	/* pointer size: 8 */
	sizeof(arri2Num); /* pointer size: 8 */
	sizeof(piNum); 		/* pointer size: 8 */
}

int main()
{
	char arrcCh[4]; 			/* statically allocated - stack*/
	int arri2Num[3][4]; 	/* statically allocated - stack */
	char* piNums = (char*) malloc(sizeof(char) * 4); 
	/* dynamically allocated - heap */

	sizeof(arrcCh); 	/* 4 */
	sizeof(arri2Num);	/* 48 */
	sizeof(piNum); 		/* pointer size: 8 */

	func(arrcCh, arri2Num, piNum);

	return 0;
}
```
{% endtab %}

{% tab title="sizeof advanced" %}
```c
#include <stdio.h>
#include <stdlib.h>

void func1(char** arri2dNum)
{
}

void func2(char* arri2dNum[5])
{
}

void func3(char arri2dNum[][5])
{
}

void func4(char arri2dNum[10][5]) /* first dimension is optional */
{
}

int main()
{
    // GOLDEN RULE: & and sizeof treat arrays as arrays, everything else treats arrays as pointers (const pointers)
    
    char a[5]; 		    // a's type: char[5], remember &a's type is char (*)[5], everywhere else a's type is pointer except with sizeof(...)
    char *b; 		      // b's type: char* (pointer to char)
    char (*c)[5];     // c's type: char (*)[5] (pointer to an array of 5 chars)
    char (*d)[5] = &a;
    char *e[5];	
    char **f;
    
    func1(&a); /* WARNING: incompatible type */
    func2(&a); /* WARNING: incompatible type */
    func3(&a);
    func4(&a);
    
    printf("sizeof(a): %ld\n", sizeof a); /* sizeof(...) brackets are optional */
    printf("sizeof(b): %ld\n", sizeof(b));
    printf("sizeof(c): %ld\n", sizeof(c));
    printf("sizeof(*c): %ld\n", sizeof(*c));
    printf("sizeof(char(*)[5]): %ld\n", sizeof(char (*)[5]));
    
    b = a; 	// a is treated as pointer here so it becomes a pointer to the first element
    c = &a; // address of array has the type char (*)[5], not char**. 
    // a is NOT treated as a pointer but an array when & is used. &a and c are both of type char(*)[5]
    printf("sizeof(char(*)[5]), sizeof(c), sizeof(&a): %ld %ld %ld\n", sizeof(char (*)[5]), sizeof(c), sizeof(&a));
    // f = &a; /* WARNING: Type mismatch. (f: char**, &a: char (*)[5] - pointer to array type) */
    // e = &a; /* WARNING: Type mismatch. (e: char**, &a: char (*)[5] - pointer to array type) */
    // e[0] = &a; /* WARNING: Type mismatch. (e[0]: char*, &a: char (*)[5] - pointer to array type) */
    
    
    printf("b == c: %d\n", (b == (char*)c)); /* or */
    printf("b == c: %d\n", ((char (*)[5])b == c));
    // b and c have their values the same (memory address of first element of array a), 
    // but b's and c's types are different. Needs casting to avoid warnings
    
    // b = &a; // WARNING: Type mismatch (b: char* - pointer type, &a: char (*)[5] - pointer to array type)
    // c = a; 	// WARNING: Type mismatch (c: char (*)[5] - pointer to array type, a: char* - pointer type), 
    // // note that & is NOT used here. a is of type char* when used with =
    
    b = (char *)malloc(5); /* Casting is optional (refer to week 03 "practical c slides" on blackboard). */
    c = (char (*)[5])malloc(5); // No problem here. But not seen quiet often. */
    
    // These are always the same no matter what's stored in the variable. 
    // Remember type of the variable does not change, and sizeof(...) is a compile time operator.
    // sizeof a; // 5, sizeof treats a as an array
    // sizeof a[0]; // 1, sizeof(char) is 1; dereferencing a, gives char    
    // sizeof b; // 8, size of a char*
    // sizeof c; // 8, pointer to char[5]
    // sizeof *c; // 5, dereferencing char(*)[5] gives char[5]. The type itself has information about the no of elements of the array.
    // sizeof c[0]; // same as above but it's a silly way to write it
    
    // sizeof(char[4][5]); // 20, sizeof treats arrays as arrays, not as pointers
    // sizeof(char[5]); // 5, sizeof treats arrays as arrays
    
    char g[4][5];
    printf("sizeof(f): %ld\n", sizeof(g)); // 20
    printf("sizeof(f[0]): %ld\n", sizeof(g[0])); // 5
    
    return 0;
}
```
{% endtab %}
{% endtabs %}

### Advanced examples of 1d, 2d arrays:

{% tabs %}
{% tab title="main" %}
```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	/* 
	******************************************************************************************************************
	In C, array declarations and initialization:
	******************************************************************************************************************
	- The size of the first dimension is optional.
	- Multi-dimensional arrays: Except the first dimension, the sizes for other dimensions must be specified.
	*/ 

	int arrNum[10]; 
	int arrNum2[] = {34, 23, 45};
	int arrNum3[5] = {34, 23, 45};

	int arr2dNum[2][3];									/* declaration only; need all dimensions */
	int arr2dNum2[][3] = {{1, 2, 3}, {3, 4, 5}}; 		/* no of rows are optional. no of columns mandatory */		
	int arr2dNum3[3][3] = {{1, 2, 3}, {3, 4}}; 		/* no of rows are optional. no of columns mandatory */	
	/*int arr2dNum4[][] = {{1, 2, 3}, {3, 4, 5}};*/		/* ERROR!, all dimensions except the first are mandatory */

	int* arrpiNum[4];
	int** arrppiNum[4];
	int** arrppiNum2[2][4];

	/* Note: sizeof() works at compile-time */
	printf("arrNum: %lu\n", sizeof(arrNum));		/* For stack allocated array: sizeof() can determined the size at compile time */
	printf("arrNum2: %lu\n", sizeof(arrNum2));		
	printf("arrNum3: %lu\n", sizeof(arrNum3));		
	printf("arr2dNum: %lu\n", sizeof(arr2dNum));		/* 2D array of integers */
	printf("arr2dNum2: %lu\n", sizeof(arr2dNum2)); 	
	printf("arr2dNum3: %lu\n", sizeof(arr2dNum3)); 
	printf("arrpiNum: %lu\n", sizeof(arrpiNum));		/* 1D array of pointers */
	printf("arrppiNum: %lu\n", sizeof(arrppiNum));		/* 1D array of double pointers */
	printf("arrppiNum2: %lu\n", sizeof(arrppiNum2));	/* 2D array of double pointers */

	/* functions are invoked at run time */
	testArrParam(arrNum);
	testArrParam2(arrNum);
	testArrParam3(arrNum);

	test2DArrParam(arr2dNum);
	test2DArrParam2(arr2dNum);
	/*test2DArrParam3(arr2dNum);*/	/* WARNING: incompatible pointer type. (Refer to function for more details) */
	/*test2DArrParam4(arr2dNum);*/	/* WARNING: incompatible pointer type. (Refer to function for more details) */

	return 0;
}
```
{% endtab %}

{% tab title="1d array parameter" %}
```c
/* 
******************************************************************************************************************
In C, array parameters in a function:
******************************************************************************************************************
- The size of the first dimension is optional. Compiler ignores it even if the correct or incorrect size is specified.
- Passing multi-dimensional arrays: Except the first dimension, the correct sizes for other dimensions must be specified. 
(if not, WARNING: expected ‘int (*)[10]’ but argument is of type ‘int (*)[3]’)
*/ 

void testArrParam(int arrNum[])
{
	/* 
	The param int arrNum[]
	--------------------------
	- is a 1D array
	- single '[]' with or without size is ignored by compiler and creates int* internally
	*/

	/* 
	sizeof(arrNum)
	----------------
	- bracket '[]' notation on the parameter arrNum triggers the WARNING: ‘sizeof’ on array function parameter ‘arrNum’ will 		return size of ‘int *’
	- sizeof works at compile-time and testArrParam(...) invocation with the argument happens at runtime.
	*/

	/*printf("testArrParam: arrNum: %lu\n", sizeof(arrNum));*/ /* WARNING above */

	int iNum;
	arrNum = &iNum; /* this is allowed since arrNum is defined to be int* internally but not int* constant pointer */
}

void testArrParam2(int arrNum[5])
{
	/* 
	The param int arrNum[]
	--------------------------
	- is a 1D array
	- single '[]' with or without size is ignored by compiler and creates int* internally
	*/

	/* 
	sizeof(arrNum)
	----------------
	- bracket '[]' notation on the parameter arrNum triggers the WARNING: ‘sizeof’ on array function parameter ‘arrNum’ will 		return size of ‘int *’
	- sizeof works at compile-time and testArrParam2(...) invocation with the argument happens at runtime.
	*/

	/*printf("testArrParam2: arrNum: %lu\n", sizeof(arrNum));*/ /* WARNING above */
}

void testArrParam3(int* piNum)
{
	/* piNum is a regular pointer here. */
	printf("testArrParam3: piNum: %lu\n", sizeof(piNum)); /* sizeof(...), NO WARNING */
}
```
{% endtab %}

{% tab title="2d array parameters" %}
```c
void test2DArrParam(int arr2dNum[10][3]) /* incorrect or correct first dimension size is ignored */
{
	/* 
	The param int arr2dNum[][3]
	--------------------------
	- is a 2D array
	- the first '[]' with or without size is ignored by compiler and creates int (*)[3] internally
	*/

	/* 
	sizeof(arr2dNum)
	----------------
	- bracket '[]' notation on the parameter arr2dNum triggers the WARNING: ‘sizeof’ on array function parameter ‘arr2dNum’ will 		return size of ‘int **’
	- sizeof works at compile-time and test2DArrParam(...) invocation with the argument happens at runtime.
	*/

	/*printf("test2DArrParam: arr2dNum: %lu\n", sizeof(arr2dNum));*/
}

/*void test2DArrParam2(int arr2dNum[][]) // must specify size of dimensions except the first
{
	printf("test2DArrParam2: arr2dNum: %lu\n", sizeof(arr2dNum));
}*/


void test2DArrParam2(int arr2dNum[][3])
{
	/* 
	The param int arr2dNum[][3]
	--------------------------
	- is a 2D array
	- the first '[]' with or without size is ignored by compiler and creates int (*)[3] internally
	*/

	/* 
	sizeof(arr2dNum)
	----------------
	- bracket '[]' notation on the parameter arr2dNum triggers the WARNING: ‘sizeof’ on array function parameter ‘arr2dNum’ will 		return size of ‘int **’
	- sizeof works at compile-time and test2DArrParam2(...) invocation with the argument happens at runtime.
	*/

	/*printf("test2DArrParam2: arr2dNum: %lu\n", sizeof(arr2dNum));*/	/* WARNING above */
}

void test2DArrParam3(int* arr2dNum[3])
{
	/* 
	The param int* arr2dNum[3]
	--------------------------
	- is a array of pointers
	- single '[]' with the size 3 is ignored by compiler and creates int** internally just like in the case of 1D arrays
	- Therefore, WARNING: expected ‘int **’ but argument is of type ‘int (*)[3]’
	*/

	/* 
	sizeof(arr2dNum)
	----------------
	- bracket '[]' notation on the parameter arr2dNum triggers the WARNING: ‘sizeof’ on array function parameter ‘arr2dNum’ will 		return size of ‘int **’
	- sizeof works at compile-time and test2DArrParam3(...) invocation with the argument happens at runtime.
	*/	

	/*printf("test2DArrParam3: arr2dNum: %lu\n", sizeof(arr2dNum));*/ 	/* WARNING above */
}

void test2DArrParam4(int** ppiNum)
{
	/* 
	The param int** ppiNum
	----------------------
	- WARNING: expected ‘int **’ but argument is of type ‘int (*)[3]’
	- In C, when invoking test2DArrParam4(...) function, 2D or Multi-dimensional array argument is considered internally as int (*)[3], <fist dimension is * always>. For i.e. single dimension argument is considered as int *	
	*/

	printf("test2DArrParam4: ppiNum: %lu\n", sizeof(ppiNum)); 		/* sizeof(...), NO WARNING */	
}
```
{% endtab %}
{% endtabs %}

## Pointer Subtraction?

pointers can be **subtracted** by another pointer of the **same type**. The resultant type is `ptrdiff_t`

```c
#include <stdio.h>
#include <string.h>
#include <stddef.h>

int main(int argc, char* argv[])
{
	int arrNum[] = {10, 20, 259};
	char* pcChar = (char*)arrNum;

	char ch = 'A';
	char* pcChar2 = &ch;

	ptrdiff_t diff = &arrNum[3] - &arrNum[1];

	/*int diff2 = &arrNum[3] - pcChar;*/ /* ERROR invalid operands to binary - (have ‘int *’ and ‘char *’) */

	/* diff3 is the offset in terms of characters (bytes). Remember pcChar, pcChar2 are char* pointers */
	ptrdiff_t diff3 = pcChar2 - pcChar; 

	/* ptrdiff_t add = &arrNum[1] + &arrNum[1];*/ /* ERROR: invalid operands to binary + (have ‘int *’ and ‘int *’) */

	printf("%ld\n", diff);
	printf("%ld\n", diff3);
	/* printf("%ld\n", add); */

	return 0;
}
```

## What are strings in C?

* In C, **string literals** "ABC" (enclosed in double-quotes) and char literals 'A' exists, but **no string datatype**.
* **strings in C are null-terminated char arrays**
* char name\[] = "ABC"; /\* name = {'A', 'B', 'C', **'\0'** }, string literals are always null-terminated. **name is a statically allocated char array**.  \*/
* char\* pcName = "ABC"; /\* points to **read only global storage** \*/

### &#x20;**Null Terminator ('\0')**

* This is not pointer NULL
* **char: '\0', octal code: '\000', hex code: '\x00', decimal: 0**

```c
/*
IMPORTANT (Null Termination) 
----------------------------
'\0' char
'\000' octal char code
'\x00' hex char code

0 decimal value for null termination
*/

void null_in_string_literals()
{
	char sName[] = "123\000567890"; /* 123<nt>567890<nt> */
	char sName2[] = "123\x00PQR"; 	/* 123<nt>PQR<nt> */
	char sName3[] = "1234567890"; 	/* 1234567890<nt> */
	sName3[3] = 0;					/* 123<nt>567890<nt> */
	
	/* sName: 123 - 3 - 11, sName2: 123 - 3 - 8, sName3: 123 - 3 - 11 */
	printf("null_in_string_literals: sName: %s - %lu - %lu, sName2: %s - %lu - %lu, sName3: %s - %lu - %lu\n", 
		sName, strlen(sName), sizeof(sName), sName2, strlen(sName2), sizeof(sName2), sName3, strlen(sName3), sizeof(sName3));
	
}
```

### What are useful functions for strings?

&#x20;**strlen(), strncpy(), strstr(), strtok() functions**

### Advanced examples of strings:

{% tabs %}
{% tab title="main" %}
```c
#include <stdio.h>
#include <string.h>

void ascii_code_in_string_literals();
void null_in_string_literals();
void printChars(char* prefix, char* arr, int len);

int main(int argc, char* argv[])
{
	/*printf("%s %d\n", argv[0], argc);*/

	char* pcSrc = "1234567890";
	char src[] = "1234567890";
	char dest[] = "Hello";	
	char dest2[15] = "XXXXXXXXXXXXXX"; 		/* <nt> is included, sizeof(<string_literal>) = 15 */	
	char dest3[15] = "XXXXXXXXXXXXXXX"; 	/* <nt> is not included, sizeof(<string_literal>) = 16, and NO WARNING !!! */
	char dest4[15] = "XXXXXXXXXXXXXXXX"; 	/* <nt> is not included, sizeof(<string_literal>) = 17, WARNING: initializer-string for array of chars is too long */
		
	printChars("pcSrc", pcSrc, sizeof("1234567890"));
	printChars("src", src, sizeof(src));
	printChars("dest", dest, sizeof(dest));
	printChars("dest2", dest2, sizeof(dest2));
	printChars("dest3", dest3, sizeof(dest3));
	printChars("dest4", dest4, sizeof(dest4));

	strncpy(dest2, src, sizeof(dest2));
	printChars("dest2", dest2, sizeof(dest2)); /* dest2 = 1234567890<nt><nt><nt><nt><nt> */
	printf("strlen(dest2): %lu\n", strlen(dest2));


	printf("\nReinitialize dest2\n");
	memset(dest2, 'X', sizeof(dest2)); 		/* Reinitialize */
	dest2[sizeof(dest2) - 1] = '\0';   		/* Set the null termination */
	printChars("dest2", dest2, sizeof(dest2));

	strncpy(dest2, src, 5);				/* Copy only 5 characters, <nt> not included */
	printChars("dest2", dest2, sizeof(dest2)); 	/* dest2 = 12345XXXXXXXXX<nt> */


	printf("\nSet <nt> in the middle of src\n");
	src[3] = '\0'; /* 123<nt>567890<nt> */
	printChars("src", src, sizeof(src)); /* dest = 123<nt>567890<nt> */

	printf("\nPerform memcpy(dest, src, sizeof(dest))\n");
	memcpy(dest, src, sizeof(dest));
	printChars("dest", dest, sizeof(dest)); /* dest = 123<nt>56, HIGH_RISK: no <nt> at the end */

	printf("\nPerform strncpy(dest, src, sizeof(dest))\n");
	strncpy(dest, src, sizeof(dest)); 
	printChars("dest", dest, sizeof(dest)); /* dest = 123<nt><nt><nt> */


	printf("\n");
	ascii_code_in_string_literals();
	return 0;
}
```
{% endtab %}

{% tab title="null_in_string_literals" %}
```c
/*
IMPORTANT (Null Termination) 
----------------------------
'\0' char
'\000' octal char code
'\x00' hex char code

0 decimal value for null termination
*/

void null_in_string_literals()
{
	char sName[] = "123\000567890"; /* 123<nt>567890<nt> */
	char sName2[] = "123\x00PQR"; 	/* 123<nt>PQR<nt> */
	char sName3[] = "1234567890"; 	/* 1234567890<nt> */
	sName3[3] = 0;					/* 123<nt>567890<nt> */
	
	/* sName: 123 - 3 - 11, sName2: 123 - 3 - 8, sName3: 123 - 3 - 11 */
	printf("null_in_string_literals: sName: %s - %lu - %lu, sName2: %s - %lu - %lu, sName3: %s - %lu - %lu\n", 
		sName, strlen(sName), sizeof(sName), sName2, strlen(sName2), sizeof(sName2), sName3, strlen(sName3), sizeof(sName3));
	
}

```
{% endtab %}

{% tab title="ascii_code_in_string_literals" %}
```c
void ascii_code_in_string_literals()
{
	/* Octal ascii code representation in C */
	/* ------------------------------------- */
	char sName[] = "\101"; 			/* max three valid octal chars (1, 2, ... 7) after '\' */
	char sName2[] = "123\101567890";
	

	/* Hex ascii code representation in C */
	/* ---------------------------------- */
	char sName3[] = "\x41";
	char sName4[] = "123\x41GHIJKL"; 		/* all valid hex chars (1, 2, ... A, ..., F) after \x */
	char sName5[] = "123\x4156";			/* WARNING: hex escape sequence out of range, but \x41-56 all numbers will 								be taken as a one hex number */
	/*
	range is: \x00 - \xff	
	*/

	/* sName: A - 2, sName2: 123A567890 - 11 */
	printf("ascii_code_in_string_literals: sName: %s - %lu, sName2: %s - %lu\n", 
		sName, sizeof(sName), sName2, sizeof(sName2)); 
	
	/* sName3: A - 2, sName4: 123A567890 - 11, sName5: 123� - 5 */
	printf("ascii_code_in_string_literals: sName3: %s - %lu, sName4: %s - %lu, sName5: %s - %lu\n", 
		sName3, sizeof(sName3), sName4, sizeof(sName4), sName5, sizeof(sName5)); 
	

	printf("\n");
	null_in_string_literals();	
}
```
{% endtab %}

{% tab title="printChars" %}
```c
void printChars(char* prefix, char* arr, int len)
{
	int i;
	printf("%s: ", prefix);
	
	for(i = 0 ; i < len ; i++)
	{
		if (arr[i] == '\0')
		{
			printf("<nt> ");
		}
		else		
		{
			printf("%c ", arr[i]);
		}
	}
	printf("- %d \n", len);	
}
```
{% endtab %}
{% endtabs %}

## **What are useful functions for malloc?**

&#x20;**memcpy(), memmove(), memset() functions**

## Can I get some hints for the practical?

Yes, here we go. But make sure to complete the code and complete the prac questions in full.

{% tabs %}
{% tab title="asc, dsc" %}
```c
#include <stdio.h>

void asc(int *piNum1, int* piNum2)
{
	if (*piNum1 > *piNum2)
	{
		/* TODO: swap the numbers pointed by piNum1, piNum2 */
	}	
}

void asc3(int* piNum1, int* piNum2, int* piNum3)
{
	printf("asc3: %d, %d, %d\n", *piNum1, *piNum2, *piNum3);
	/* DO NOT write any if conditions or while loops here */
	/* Try calling the asc(...) function 3 time to sort the 
	numbers pointed by piNum1, piNum2, piNum3 */
}

void dsc3(int* piNum1, int* piNum2, int* piNum3)
{
	printf("dsc3: %d, %d, %d\n", *piNum1, *piNum2, *piNum3);
	/* DO NOT write any if conditions or while loops here */
	/* TODO: Try calling the asc(...) function 3 time to sort the 
	numbers pointed by piNum1, piNum2, piNum3 */
}


int main()
{
	/* TODO: Take inputs choice, iNum1, iNum2, iNum3 from the user */
	char choice = 'D';
	int iNum1 = 150, iNum2 = 50, iNum3 = 100;

	void (*pFunc)(int*, int*, int*);

	if (choice == 'A')
	{
		/* Set the function pointer to asc3 */
		pFunc = &asc3;
	}
	else if (choice == 'D')
	{
		/* Set the function pointer to dsc3 */
		pFunc = &dsc3;
	}

	/* Invoke the function pointed by pFunc */
	(*pFunc)(&iNum1, &iNum2, &iNum3);

	printf("%d %d %d\n", iNum1, iNum2, iNum3);
	return 0;
}
```
{% endtab %}

{% tab title="sum, max" %}
```c
int sum(int arriNums[], int len)
{
	/* TODO: For loop, iterate each element in arriNums, add them to total */
	
	return total;
}

int max(int arriNums[], int len)
{

	int max_num = /* figure it out */
	int max_index = /* figure it out */ 
	
	// For loop, iterate each element in arriNums, keep track of the current max_num, the index of that element max_index

	int i;
	for (i = 0 ; i < len ; i++)
	{
		if (arriNums[i] > max_num)
		{
			max_num = arriNums[i];
			max_index = i;
		}
	}
	
	return max_index;
}
```
{% endtab %}

{% tab title="reverse" %}
```c
int* reverse_v1(int arriNums[], int len)
{	
	/* Easy alternative */
	int* arriNumRevered = (int*) malloc(sizeof(int) * len);

	/* TODO: In a for loop, reverse the elements, 
	set arriNumReversed elements accordingly */

	return arriNumRevered;

}

void reverse_v2(int* piNum, int len)
{
	/* This is what is expected in the prac sheet */
	
	/* TODO: In a for loop, reverse the elements in piNum */
	
}

int main()
{
	int arriNums[] = {10, 20, 30, 40};

	int* piNum = reverse(arriNums, 10);
	printf("%d", piNum[0]);	

	reverse_v2(arriNums, int len);
	printf("%d", arriNums[0]);	

	// Array of strings
	char* arrNames[3] = {"10", "100", "200"};	
	
	// Iterate array of strings and convert to numbers
	int arrNums[3];
	for (i = 0; i < 3 ; i++)
	{
		arrNums[i] = atoi(arrNames[i]);
	}

	/* TODO: prints the numbers in arrNums */

	printf("{")
	for (i = 0; i < 5 ; i++)
	{
		if(it is not the last iteration)
			printf("%d, ", arrNums[i]);
		else
			printf("%d", arrNums[i]);	
	}
	printf("}")
}
```
{% endtab %}
{% endtabs %}

## How to malloc and free 2-d array?

compile the following code with&#x20;

`gcc -Wall -ansi -pedantic -g main.c -o prog`(-g flag for additional debug information)&#x20;

and run it using `valgrind ./prog`

&#x20;Observe the valgrind report for no memory leaks

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int i, j;
    int nRows = 3, nCols = 4;
    int** arri2dNums;

    /* ------------------------------*/
    /* 01 Allocate Memory */
    /* ------------------------------*/
    /* malloc for the array of rows */
    arri2dNums = (int**) malloc(sizeof(int*) * nRows);
    
    /* malloc columns for each row */
    for (i = 0 ;  i < nRows ; i++)
    {
        arri2dNums[i] = (int*) malloc(sizeof(int) * nCols); 
    }

    /* malloc for 2d array is done. But remember 
    elements are not initialized yet */

    /* ------------------------------*/
    /* 02 Initialize Allocated Memory */
    /* ------------------------------*/
    /* Initialize the malloc-ed 2d array elements with 0 */
    for (i = 0 ;  i < nRows ; i++)
    {
        for (j = 0 ;  j < nCols ; j++)
        {
            arri2dNums[i][j] = 10;
        }
    }

    /* Print the values to validate the set values */
    for (i = 0 ;  i < nRows ; i++)
    {
        for (j = 0 ;  j < nCols ; j++)
        {
            printf("%d ", arri2dNums[i][j]);
        }

        printf("\n");
    }

    
    /* ------------------------------*/
    /* 03 Free 2d malloc-ed array */
    /* ------------------------------*/
    /* The following order of steps is mandatory in freeing the memory */
    
    /* First free the columns in each row */
    for (i = 0 ;  i < nRows ; i++)
    {
        free(arri2dNums[i]);
        arri2dNums[i] = NULL; /* Optional: but recommended to prevent unauthorized access to freed memory */
    }
    
    /* Then free the array of rows */
    free(arri2dNums);
    arri2dNums = NULL; /* Optional: but recommended to prevent unauthorized access to freed memory */
        
    /* IMPORTANT: Remember, no of mallocs = no of free s */                    
    return 0;
}
```

### Can I do allocation and initialization of the allocated memory at once without repeating the loops?

Yes, there are smart ways to achieve this:

{% tabs %}
{% tab title="Alternative 01" %}
```c
    int** arri2dNums = (int**) malloc(sizeof(int*) * nRows);
    
    /* ------------------------------*/
    /* 01 Allocate & Initialize Memory */
    /* ------------------------------*/
    /* malloc and init columns for each row */
    for (i = 0 ;  i < nRows ; i++)
    {
        arri2dNums[i] = (int*) malloc(sizeof(int) * nCols); 
        memset(arri2dNums[i], 0, sizeof(int) * nCols); /* Initialize the allocated memory */
    }
```
{% endtab %}

{% tab title="Alternative 02 (calloc)" %}
```c
    int** arri2dNums = (int**) malloc(sizeof(int*) * nRows);
    
    /* ------------------------------*/
    /* 01 Allocate & Initialize Memory */
    /* ------------------------------*/
    /* malloc and init columns for each row */
    for (i = 0 ;  i < nRows ; i++)
    {
        arri2dNums[i] = (int*) calloc(nCols, sizeof(int));        
    }
```
{% endtab %}
{% endtabs %}

### Can I expand/shrink a malloc-ed memory later dynamically?

Yes, using realloc(...). See below:

```c
void *realloc( void *ptr, size_t new_size );
```

Reallocates the given area of memory. It must be previously allocated by [malloc()](https://en.cppreference.com/w/c/memory/malloc), [calloc()](https://en.cppreference.com/w/c/memory/calloc) or `realloc()` and not yet freed with a call to [free](https://en.cppreference.com/w/c/memory/free) or `realloc`. Otherwise, the results are undefined.

The reallocation is done by either:

* expanding or contracting the existing area pointed to by `ptr`, if possible. The contents of the area remain unchanged up to the lesser of the new and old sizes. If the area is expanded, the contents of the new part of the array are undefined.
* allocating a new memory block of size `new_size` bytes, copying memory area with size equal the lesser of the new and the old sizes, and freeing the old block.

If there is not enough memory, the old memory block is not freed and a null pointer is returned.

{% tabs %}
{% tab title="Expanding Memory" %}
```c
int *array = malloc (10 * sizeof(int))
int i;
for (i = 0 ; i < 10 ; i++)
{
    array[i] = i;
}

/* Reallocate memory: Allocates a new block of
 * memory and free the old one.
 */
int *new_array = realloc(array, 15 * sizeof(int));

if (new_array == NULL) {
    perror("realloc");
    exit(1);
}

/* Now array has 15 items. */
array = new_array;
```
{% endtab %}

{% tab title="Shrinking Memory" %}
```c
int *array = malloc (10 * sizeof(int))
int i;
for (i = 0 ; i < 10 ; i++)
{
    array[i] = i;
}

/* Following program takes the last 7 items of array, shift it to the
 * begining and realloc (7 item) array with it 
 * ----------------------------------------------*/

/* Shift array entries to the left 3 spaces. Note the use of memmove
 * and not memcpy since the areas overlap.
 */
memmove(array, array + 3, 7);

/* Reallocate memory. realloc will "probably" just shrink the previously
 * allocated memory block, but it's allowed to allocate a new block of
 * memory and free the old one if it so desires.
 */
int *new_array = realloc(array, 7 * sizeof(int));

if (new_array == NULL) {
    perror("realloc");
    exit(1);
}

/* Now array has only 7 items. */
array = new_array;
```
{% endtab %}
{% endtabs %}

## Can I return a stack-allocated variable or array outside a function?

No, do not do it at all.

```c
int* getVariable()
{
    int iNum = 10;
    return &iNum;
}

int main()
{
    int* piVal = getVariable();    
    /* Once getVariable() function returns iNum defined in the 
    function locally on the stack will be destroyed */

    printf("%d \n", *piVal); /* Here, you are accessing a destroyed variable */
    return 0;
}
```

## C Strings

### What are strings in C?

Strings in C are null terminated character arrays.

```c
char str[] = {'h', 'e', 'l', 'l', 'o', '/0'};
```

### What are string literals and character literals in C?

String literals are `"abc", "A"` (enclosed in double quotes)

Character literals are `'A', 'B'` (enclosed in single quotes)

Basically, when ever you see strings in C, assume you are looking at an null terminated char array:

```c
"abc" => 'a', 'b', 'c', '\0'
"A" => 'A', '\0'
```

Please refer to the following example:

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char ch = 'A'; /* Single quotes for char literal */
    char ch2 = "A"; /* ERROR! trying to assign char variable with a string literal */
    char str[] = "A"; /* Double quotes, equivalent to {'A', '\0'} */
    char str[2] = "A" /* Same as above */
    
    /*
    More example are provided in 
    Blackboard | Learning Materials -> Week 04 Module -> Prac Reference Section -> Practical C slides
    */
    
    
    if (ch == 'A') /* no problem here */
    {
        printf("ch is A");
    }
    
    if (ch == "A") /* ERROR: comparing a char variable with a string literal */
    {
        printf("ch is A");
    }
    
    if (strcmp(str, "A") == 0) /* Comparing strings (null terminated char array element-wise using strcmp method */
    {
        printf("str is A");
    }       
    
    return 0;
}
```

### What are string gloabl pool allocations?

Strings that are already allocated in read-only global storage pool in the memory.

```c
char str[] = "Hello"; /* Allocation happens in stack */
char* str2 = "Hello"; /* Allocation has already happened in read-only
                         global storage pool. str2 is just a pointer
                         to the first element of the array "Hello" */
                                
str[0]= 'h'; /* allowed */
str2[0] = 'h'; /* not allowed since the str2 is pointing to an array allocated in 
                  read-only global storage */
```

## How to find more examples on Arrays / Strings / Function Pointers

Please refer to:&#x20;

```c
Blackboard | Learning Materials -> Week 03/04/05 Module -> Prac Reference Section -> Practical C slides
```

Also, refer to the practical recording via collab ultra for explanations of the slides.
