# Conditional Debugging

## Enabling simple conditional debugging with #ifdef

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

