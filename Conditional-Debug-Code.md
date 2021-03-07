# Conditional Debugging

This article discusses the use of conditional compilation for enabling or disabling debug code in your program. Conditional compilation directives allow you to designate parts of the program for the compiler to ignore. These parts are not compiled and no object code is generated for them.The test performed by a conditional directive is done at compile-time by the compiler. Depending on the result of the test, source code is either included or excluded from the compilation. There are several reasons to use conditional compilation in your program.

* program may require different code depending on which device or architecture you use. In some cases, library routines may exist in one configuration that do not exist in another. Conditional compilation allows you to handle such a situation by substituting alternate functions with the unavailable library routines.
* Many complex functions require comprehensive test code to verify or validate I/O and proper operation. Intermediate values may be tested and output as well. After verification, you may wish to retain the test cases for future reference. You can include them in conditional blocks you control with a macro:

```C
#define DEBUG 0
```

The following test case compiles only when DEBUG is defined to a value other than 0.

```C
#if DEBUG   /*** Test Case ***/
.
.
.
#endif
```

Often, it is necessary to replace or rewrite certain sections of a working program. Conditionals allow you to retain old code in the source file by placing it in a conditional block. For example:

```C
#if 0   /*** Old code from 1999 ***/
.
.
.
#endif
```

The ```#ifdef``` directive is a special case of the ```#if``` directive that tests a name to determine if it is defined.

```#ifdef name```

This directive is equivalent to:

```#if defined(name)```

```#ifndef``` - same as ```#ifdef``` but the evaluation succeeds if the definition is not defined.

#ifndef name

This directive is equivalent to:

```#if !defined(name)```

## Reorganize these links in prority order

https://www.keil.com/support/man/docs/c51/c51_pp_conditionals.htm  
https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html
https://stackoverflow.com/questions/26396915/gcc-debug-flags-scons
https://stackoverflow.com/questions/2754966/cflags-vs-cppflags
https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html
https://en.wikipedia.org/wiki/CFLAGS
https://www.eskimo.com/~scs/cclass/notes/sx9c.html  
https://www.learncpp.com/cpp-tutorial/more-debugging-tactics/  
https://cboard.cprogramming.com/c-programming/153836-what-exactly-does-sharpifdef-debug-do.html  
http://boost-log.sourceforge.net/libs/log/doc/html/index.html
https://stackoverflow.com/questions/24195123/using-define-in-c-with-no-value  
https://stackoverflow.com/questions/1079832/how-can-i-configure-my-makefile-for-debug-and-release-builds  
https://stackoverflow.com/questions/25797287/how-do-i-add-a-debug-option-to-makefile/25801355  
http://web.mit.edu/rhel-doc/3/rhel-gcc-en-3/debugging-options.html  
https://stackoverflow.com/questions/6465557/where-is-debug-defined-in-the-code 
https://stackoverflow.com/questions/2290509/debug-vs-ndebug  
https://bytes.com/topic/c/answers/130027-difference-between-debug-ndebug
https://stackoverflow.com/questions/28737776/standard-way-for-writing-a-debug-mode-in-c  
https://stackoverflow.com/questions/41320050/ddebug-0-doesnt-work

## 1. Enabling simple conditional debugging

```C

/***************************************************************** 
* simpleDebug.c                                                  *
* Either of these three 'defines will have the same effect,      * 
* the #ifdef pre-processor only looks whether DEBUG is defined,  *
* and does not really care about its value.                      *
******************************************************************/ 

#include <stdio.h>
#include <stdlib.h>

#define DEBUG 
// #define DEBUG 1
// #define DEBUG 0

int main(int argc, char *argv[])
{
  
  #ifdef DEBUG
  printf("main()...\n");
  #endif

  printf("Hello Debug!\n");
  
  #ifdef DEBUG
  printf("main() returning...\n");
  #endif

  return 0;
}

```

Now compile this file as follows:

```Bash
gcc -o simpleDebug simpleDebug.c
```

When you run:

```Bash
./simpleDebug
```

You should see:

```Bash
main()...
Hello Debug!
main() returning...
```

If you comment out the ```#define``` for ```DEBUG``` and then compile, you will not see the debug code being printed:

```Bash
Hello Debug!
```

## 2. Enabling debug by passing debug variables to gcc during compilation:
If you just pass the debug variable as ```D<DEBUGVAR>``` which in this case is ```DDEBUG``` to gcc during compilation, then debug code is compiled, otherwise it gets ommitted during the pre-processing step. For example:

```C

/***************************************************************** 
* gccDebug.c                                                     *
* enable or disable conditional debug based on flags passed      *
* to the compiler.                                               *
******************************************************************/ 

#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
  
  #ifdef DEBUG
  printf("main()...\n");
  #endif

  printf("Hello Debug!\n");
  
  #ifdef DEBUG
  printf("main() returning...\n");
  #endif

  return 0;
}

```
to enable debug statements compile your source as follows:

```Bash
gcc -DDEBUG -o simpleDebug simpleDebug.c
```

to disable debug statements do not pass the debug flag DDEBUG to gcc:

```Bash
gcc o simpleDebug simpleDebug.c
```

## 3. Conditional debug by using debug print macros

```C

/***************************************************************** 
* gccMacroDebug.c                                                *
* enable or disable conditional debug based on flags passed      *
* to the compiler, and debug statements defined as macros.       *
******************************************************************/ 

#include <stdio.h>
#include <stdlib.h>

#ifdef DEBUG
  #define DEBUG_PRINT(X) printf(X)
#else
  #define DEBUG_PRINT(X)
#endif

int main(int argc, char *argv[])
{
  
  DEBUG_PRINT("main()...\n");

  printf("Hello Debug!\n");
  
  DEBUG_PRINT("main() returning...\n");

  return 0;

}

```

To enable debugging compile your code as:

```Bash
gcc -DDEBUG -o simpleDebug simpleDebug.c
```
