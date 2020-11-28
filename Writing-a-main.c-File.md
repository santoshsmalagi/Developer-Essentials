# Structuring a Multi-File C Program

## Writing a Good main() Function

This is a short summary of the article ["How to write a good C main function"](https://opensource.com/article/19/5/how-write-good-c-main-function). It tries to address the following questions:

* How to structure a C file and write a ```main()``` function
* How to best process command line arguments

Typical structure of a ```main()``` function in a C program:

```C
/* main.c */
int main(int argc, char *argv[]) {

}
```

**What is the main()?**  
Every C program MUST have only one ```main()``` function and program execution begins at the ```main()```. The ```main()``` function has two arguments that traditionally are called ```argc``` and ```argv``` and always returns a signed integer.  ```main()``` returns a 0 (zero) on success and -1 (negative one) on failure.

| Argument | Name            | Description                   |
|----------|-----------------|-------------------------------|
| argc     | argument count  | Length of the argument vector |
| argv     | argument vector | Array of char pointers        |

The argument vector - argv, is a tokenized representation of the command line that invoked the program. The argument vector is guaranteed to always have at least one string in the first index, argv[0], which is the full path to the program executed. For example if ```a.out``` be the program being executed and it is passed the following command line arguments:

```Console
$:~ a.out foo 28 M
```
Then ```argv = ["/home/malagi/a.out", "foo" "28"]``` and ```argc=3```.

### Structure of a good main.c

A good outline for ```main.c``` looks somehting like this:

```C
/* main.c */
/* 0 copyright/licensing */
/* 1 includes */
/* 2 defines */
/* 3 external declarations */
/* 4 typedefs */
/* 5 global variable declarations */
/* 6 function prototypes */

int main(int argc, char *argv[]) {
/* 7 command-line parsing */
}

/* 8 function declarations */
```

It is always a good practice to add meaningful comments, do not write about what the code is doing - instead, write about why the code is doing what it's doing!

### 0. Copyright and Licensing
Usually this is some form of standard template text which describes information such as the autor, organization, version information, copyright notice etc. It may also be helpful to briefly describe the intended purpose of this C file, especially if the program binary is a part of collection of several binaries, or the file itself is not named as main.c

### 1.Includes
The first things to add to a main.c file are includes to make a multitude of standard C library functions and variables available to the program. The #include string is a C preprocessor (cpp) directive that causes the inclusion of the referenced file, in its entirety, in the current file. Header files in C are usually named with a .h extension and should not contain any executable code; only macros, defines, typedefs, and external variable and function prototypes. The string <header.h> tells cpp to look for a file called header.h in the system-defined header path, usually /usr/include. At a minimum the following are recommended to be included in the main.c file:

```C
/* main.c */
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <libgen.h>
#include <errno.h>
#include <string.h>
#include <getopt.h>
#include <sys/types.h>
```

| Header File | Contents                                                                     |
|-------------|------------------------------------------------------------------------------|
| stdio       | Supplies FILE, stdin, stdout, stderr, and the fprint() family of   functions |
| stdlib      | Supplies malloc(), calloc(), and realloc()                                   |
| unistd      | Supplies EXIT_FAILURE, EXIT_SUCCESS                                          |
| libgen      | Supplies the basename() function                                             |
| errno       | Defines the external errno variable and all the values it can take on        |
| string      | Supplies memcpy(), memset(), and the strlen() family of functions            |
| getopt      | Supplies external optarg, opterr, optind, and getopt() function              |
| sys/types   | Typedef shortcuts like uint32_t and uint64_t                                 |
