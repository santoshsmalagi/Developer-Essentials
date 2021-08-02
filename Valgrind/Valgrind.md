# Valgrind

Valgrind is a tool to check for memory errors such as memory leaks. Consider the following simple example:

```C

#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
  int *ptr;
  ptr = (int*)malloc(sizeof(int));
  *ptr = 2;
  printf("\n Value:%d",*ptr);
  return 0;
}

```

In the above example, the ```ptr``` is assigned a memory location using ```malloc```, but this does not get freed at the end. To check for error use Valgrind:

```console
user@VM:~$ gcc -g -Wall -o MemoryLeak MemoryLeak.c
user@VM:~$ valgrind -v ./MemoryLeak 
```

For more detailed information:
```console
user@VM:~$ valgrind -v --leak-check=full --leak-resolution=high --track-origins=yes ./MemoryLeak
```

Valgrind reports the memory leak hapening on line 7.
