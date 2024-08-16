---
description: Other coding / compilation / linking issues
---

# Misc Bugs

## WARNING: ISO C90 forbids mixed declarations and code \[-Wdeclaration-after-statement]

All declarations had to come before any statements in a function code block.

{% tabs %}
{% tab title="Warning" %}
```c
int main()
{ /* open curly brace indicates the start of function code block */

int num1 = 10;
printf("%d", num1);

int num2 = 10; /* WARNING: mixed declrations...  */
printf("%d", num2);

} /* close curly brace indicates the end of the function code block */
```
{% endtab %}

{% tab title="Fix" %}
```c
int main()
{ /* open curly brace indicates the start of function code block */

int num1 = 10;
int num2 = 10; /* move all the declarations to the top */

printf("%d", num1);
printf("%d", num2);

} /* close curly brace indicates the end of the function code block */
```
{% endtab %}
{% endtabs %}

## Why is my source code converted to a weird set of symbols (machine code: non-printable characters) after compilation?

> After the compilation with the following command, my source code has  turned into something which I cannot read
>
> ```
> gcc -Wall -ansi -pedantic main.c -o main.c
> ```

`gcc` utility compiles the source code (`main.c`) into an executable which is something (binary/machine code) understandable by the machine. We call it the executable file. Therefore, it is important to provide a name to the output file (executable file) which is different to the source file during compilation. Otherwise, the executable file will replace the source file which indeed is not human readable. So the fix is:

```c
gcc -Wall -ansi -pedantic main.c -o prog
```

main.c : source file\
prog : executable file

## Makefile compilation fails with a weird error?

![screenshot](../.gitbook/assets/wierd\_link\_error.png)

It seems to be a linker error. I believe you are running off from OneDrive share straight away. What you can do is, move your code to home directory and try compiling the code with gcc -c first and then link it using gcc -o to create the executable. If this works (both the compilation and linking), use make file to compile and link. It should work.

It could also be something related to the object file not being compatible with the host machine you are trying to run on. This can happen if you compile the code on one machine and try to do the next step on another machine environment. Remember, compiled code **(.o) files** **are platform dependant**.

Try removing all the object files and start compiling from scratch again from your computer.

## Weird linked failed / permission issue when trying to compile the code?

> I am using VMware curtin desktop Computer Science Linux.\
> When trying to run this command in the terminal`gcc -Wall -ansi -pedantic Q2.c -o Q2`\
> \
> I get this error below
>
> `/usr/bin/ld: final link failed: Operation not supported`\
> `collect2: error: ld returned 1 exit status`
>
> How do I fix this so I can compile my c files properly?

It might be because of the permission issue. Try to copy the code to the Desktop directory and compile it there :)

You also can try to run this command to set the permissions to execute the program.

`chmod 700 ./q2`
