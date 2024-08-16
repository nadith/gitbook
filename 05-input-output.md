---
description: File input/output operations
---

# 05 - Input / Output

## Can I have some examples of conversion specifiers/format specifiers for printf?

The following example summarizes the use of conversion specifiers for printf. Note the C89 support

```c
#include <stdio.h>

int main()
{
	char cCh = 'A';
	short shNum = 65;
	int iNum = 97;
	long lNum = 98;
	float fNum = 1024.5;
	double dNum = 1000024.5555;

	printf("%%c - char: %c\n", cCh);
	printf("%%c - short: %c\n", shNum);
	printf("%%c - int: %c\n", iNum);
	printf("%%c - long: %c\n", lNum); 		/* WARNING */

	printf("%%d - char: %d\n", cCh);
	printf("%%d - short: %d\n", shNum);
	printf("%%d - int: %d\n", iNum);
	printf("%%d - long: %d\n", lNum); 		/* WARNING */

	printf("%%ld - char: %ld\n", cCh); 		/* WARNING */
	printf("%%ld - short: %ld\n", shNum); /* WARNING */
	printf("%%ld - int: %ld\n", iNum); 		/* WARNING */
	printf("%%ld - long: %ld\n", lNum); 

	printf("%%f - int: %f\n", iNum); 			/* WARNING */
	printf("%%f - float: %f\n", fNum); 	
	printf("%%f - double: %f\n", dNum); 

	printf("%%lf - double: %lf\n", dNum); /* %lf WARNING: no support in C89 for printf, but scanf */

	return 0;
}
```

## Why does %d in printf supports char, short, int, but in scanf only int\* ?

scanf expects a pointer type (int\*) with %d unlike a typical variable in printf. Therefore, in scanf, with each conversion specifier there will be only one pointer type associated

i.e.

%d - with int\* and only with int\* (not char\* or short\*)

%c - char\*

%f - float\*

%lf - double\*



On the other hand with printf

%d - char, short, int (implicit casting happens from smallar data such as char, short types to int)

%f - float, double



