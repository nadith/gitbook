---
description: UCP expectations and mandatory requirements.
---

# UCP EXPECTATION

## Mandatory Requirements:

### Use C89 Standard

> C89 is the first standard of the C language and there has been multiple revisions to C89 such as C99, etc. In UCP,  **DO NOT USE** any other standards of C such as C99, C++, C++.NET, etc. Most internet resource may provide you code snippets that are compliant with C99. \
> Therefore, make sure to use `-ansi -pedantic` flags (to check whether it's C89 compliant) along with `-Wall` flag (to show all warnings) during the compilation of those code snippets. Refer to [this link](https://curtin-ucp.gitbook.io/faq/01-c-basics#gcc-wall-ansi-pedantic-werror-what-are-these-flags) for more details on flats. Futhermore, some of differences between C89 and C99 are listed [here](https://curtin-ucp.gitbook.io/faq/01-c-basics)

### Use gcc (GNU C Compiler) For Code Compilation

> `gcc` is a one of the well known, well accepted compiler for C code compilation. It's a mature compiler that could perform code optimization during C code compilation process. In UCP, **DO NOT USE** any other compilers such as `clang`, `cc`, etc.

### Use make Utility to Compile your Code Base

> `make` utility is used to compile multiple C files (your code base) at once and it has many advantages. Using `make` will only perform `gcc` compilation on those files that are changed from the most recent/previous build. In UCP, **we strongly recommend** to use the make utility to compile your code base.

### Use gdb (GNU Debugger) For Runtime Debugging

> `gdb` is a one of the well known, well accepted debugger for C program debugging. In UCP, **DO NOT USE** any other debuggers.

### Use valgrind For Memory Issue Debugging

> `valgrind`is a one of the well known tools to find memory access errors to heap memory along with some other memory related issues. In UCP, it is **strongly advised** to use this tool to assess your code against the memory issues.

### Use (VS Code, vim, Notepad / Notepad++ /Text Editor) For C Source Codes

> Use one of the above editors (prefereably VSCode) during C programming to edit your source codes. In UCP, VS Code will be used extensively for its intelligent features and ease of use. More advanced features of VS Code will be discussed in later weeks to make C programming easy and more interesting!
>
> **Special Note:** During the first few weeks, please use VS Code or vim as a dumb notepad editor and use a terminal to compile your code and run it. Later on, you can try the advanced features of VS Code with some experience on the complete process of C code compilation and debugging with `gcc` and `gdb`.
>
> In UCP, you are expected to know `gcc, gdb, valgrind, make` utilities usage on a terminal in detail. Therefore, it is important that you do not rely on VS Code advance features to automate those tasks.&#x20;

### Before an Assignment Submission

> Make sure your code compiles with `make` utility and runs **on Linux platform (Lab PC or Ubuntu 18.04.5 LTS / VMWare Horizon Server) without any warnings or errors.**
>
> _Compilable code in any other platform such as Windows, MAC, etc. will NOT be taken into consideration during marking!_&#x20;
