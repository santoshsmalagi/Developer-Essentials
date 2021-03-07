# Conditional Debugging

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












