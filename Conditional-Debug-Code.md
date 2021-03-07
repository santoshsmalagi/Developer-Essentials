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

## 1. Enabling simple conditional debugging with #ifdef

```C

#include <stdio.h>
#include <stdlib.h>

/***************************************************************** 
* simpleDebug.c                                                  *
* Either of these three 'defines will have the same effect,      * 
* the pre-processor only looks whether DEBUG is defined,         *
* and does not really care about its value.                      *
******************************************************************/ 

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

## 1. Enabling debug by passing debug variables to gcc during compilation:
If you just pass the debug variable as ```D<DEBUGVAR>``` which in this case is ```DDEBUG``` to gcc during compilation, then debug code is compiled, otherwise it gets ommitted during the pre-processing step. For example:


```C

#include <stdio.h>
#include <stdlib.h>

/***************************************************************** 
* gccDebug.c                                                     *
* enable or disable conditional debug based on flags passed      *
* to the compiler.                                               *
******************************************************************/ 

#define DEBUG 

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












