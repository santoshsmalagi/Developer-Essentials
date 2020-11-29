# Writing a Good main() Function

The contents of this page are based on the original article which appeared on Opensource.com - ["How to write a good C main function"](https://opensource.com/article/19/5/how-write-good-c-main-function). It tries to address the following:

* How to structure a C file containing a ```main()``` function that will be easy to maintain
* How to best process command line arguments
* The main() function should only act as a facilitator and perform these three tasks:
  * parse the arguments 
  * perform minimal input validation 
  * pass the collected arguments to functions that will use them

**What is the main()?**  
Every C program MUST have only one ```main()``` function and program execution begins at the ```main()```. The ```main()``` function has two arguments that traditionally are called ```argc``` and ```argv``` and always returns a signed integer.  ```main()``` returns a 0 (zero) on success and -1 (negative one) on failure.

```C
/* main.c */
int main(int argc, char *argv[]) {

}
```

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

A good outline for ```main.c``` looks like something like this:

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

Additionally it is always a good practice to add meaningful comments. Do not write about what the code is doing - instead, write about why the code is doing what it's doing!

### 0. Copyright and Licensing
Usually this is some form of standard template text which describes information such as the autor, organization, version information, copyright notice etc. It may also be helpful to briefly describe the intended purpose of this C file, especially if the program binary is a part of collection of several binaries, or the file itself is not named as main.c but contains the main() function.

### 1.Includes
The first things to add to a main.c file are includes to make a multitude of standard C library functions and variables available to the program. The #include string is a C preprocessor (cpp) directive that causes the inclusion of the referenced file, in its entirety, in the current file. At a minimum the following are recommended to be included in the main.c file:

```C
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


### 2. Defines

```C
#define OPTSTR "vi:o:f:h"
#define USAGE_FMT  "%s [-v] [-f hexflag] [-i inputfile] [-o outputfile] [-h]"
#define ERR_FOPEN_INPUT  "fopen(input, r)"
#define ERR_FOPEN_OUTPUT "fopen(output, w)"
#define ERR_DO_THE_NEEDFUL "do_the_needful blew up"
#define DEFAULT_PROGNAME "george"
```

Constants - int, char, float and string contants should be defined using #defines in this part of the file. Collecting them at a single location makes it easier to fix spelling, reuse messages, and internationalize messages, if required.  Use all capital letters when naming a #define to distinguish it from variable and function names. The define names can be a single continuous string or they can be separated with an underscore; just make sure they're all upper case.


### 3. External Declarations

An extern declaration brings that name into the namespace of the current compilation unit (aka "file") and allows the program to access that variable.

```C
extern int errno;
extern char *optarg;
extern int opterr, optind;
```

### 4. typedef's

```C
typedef struct {
  int           verbose;
  uint32_t      flags;
  FILE         *input;
  FILE         *output;
} options_t;
```

After external declarations, declare typedef's for structures, unions, and enumerations. For naming a typedef prefer a \_t suffix to indicate that the name is a type. Since C is a whitespace-neutral programming language, whitespaces can be used to line up field names in the same column. For pointer declarations, prepend the asterisk to the name to make it clear that it's a pointer.

### 5. Global Variable Declarations

```C
int dumb_global_variable = -11;
```

Global variables are a bad idea and you should never use them. But if you have to use a global variable, declare them here and be sure to give them a default value. Seriously, don't use global variables.

### 6. Function Prototypes

```
void usage(char *progname, int opt);
int  do_the_needful(options_t *options);
```
A good practice (or choice of style) is to define the functions after the ```main()``` and not before. So the function prototypes need to be declared here. Early C compilers used a single-pass strategy, which meant that every symbol (variable or function name) you used in your program had to be declared before you used it. Modern compilers are nearly all multi-pass compilers that build a complete symbol table before generating code, so using function prototypes is not strictly required.

### 7. The Actual main() - (1) parse the arguments (2) perform minimal input validation (3) pass the collected arguments to functions that will use them

The purpose of the main() function is to collect the arguments that the user provides, perform minimal input validation, and then pass the collected arguments to functions that will use them. This example declares an options variable initialized with default values and parse the command line, updating options as necessary.

```C

int main(int argc, char *argv[]) {
    int opt;
    options_t options = { 0, 0x0, stdin, stdout };

    opterr = 0;

    while ((opt = getopt(argc, argv, OPTSTR)) != EOF)
       switch(opt) {
           case 'i':
              if (!(options.input = fopen(optarg, "r")) ){
                 perror(ERR_FOPEN_INPUT);
                 exit(EXIT_FAILURE);
                 /* NOTREACHED */
              }
              break;

           case 'o':
              if (!(options.output = fopen(optarg, "w")) ){
                 perror(ERR_FOPEN_OUTPUT);
                 exit(EXIT_FAILURE);
                 /* NOTREACHED */
              }    
              break;
             
           case 'f':
              options.flags = (uint32_t )strtoul(optarg, NULL, 16);
              break;

           case 'v':
              options.verbose += 1;
              break;

           case 'h':
           default:
              usage(basename(argv[0]), opt);
              /* NOTREACHED */
              break;
       }

    if (do_the_needful(&options) != EXIT_SUCCESS) {
       perror(ERR_DO_THE_NEEDFUL);
       exit(EXIT_FAILURE);
       /* NOTREACHED */
    }

    return EXIT_SUCCESS;
}
```

The guts of this main() function is a while loop that steps through argv looking for command line options and their arguments (if any). When a known command line option is detected, option-specific behavior happens. Some options have an argument, when an option has an argument, the next string in argv is available to the program. Files are opened for reading and writing or command line arguments are converted from a string to an integer value.

### 8. Function Definitions

```C
void usage(char *progname, int opt) {
   fprintf(stderr, USAGE_FMT, progname?progname:DEFAULT_PROGNAME);
   exit(EXIT_FAILURE);
   /* NOTREACHED */
}

int do_the_needful(options_t *options) {

   if (!options) {
     errno = EINVAL;
     return EXIT_FAILURE;
   }

   if (!options->input || !options->output) {
     errno = ENOENT;
     return EXIT_FAILURE;
   }

   /* XXX do needful stuff */

   return EXIT_SUCCESS;
}
```

Finally, write functions that aren't boilerplate. Functions should almost always validate their input in some way. If full validation is expensive, try to do it once and treat the validated data as immutable. 

The big class of errors to avoid is de-referencing a NULL pointer. This will cause the operating system to send a special signal called SYSSEGV, which results in unavoidable death. The last thing users want to see is a crash due to SYSSEGV. It's much better to catch a NULL pointer in order to emit better error messages and shut down the program gracefully.

A good practice is that - if something goes wrong in the middle of a function, it's a good time to return an error condition. Writing a ton of nested if statements to just have one return is never a "good idea. Finally, if you write a function that takes four or more arguments, consider bundling them in a structure and passing a pointer to the structure. This makes the function signatures simpler, making them easier to remember and not screw up when they're called later. It also makes calling the function slightly faster, since fewer things need to be copied into the function's stack frame. In practice, this will only become a consideration if the function is called millions or billions of times. Don't worry about it if that doesn't make sense.

Putting it all together:

```C
/* main.c - Command line processing example*/

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <libgen.h>
#include <errno.h>
#include <string.h>
#include <getopt.h>

#define OPTSTR "vi:o:f:h"
#define USAGE_FMT  "%s [-v] [-f hexflag] [-i inputfile] [-o outputfile] [-h]"
#define ERR_FOPEN_INPUT  "fopen(input, r)"
#define ERR_FOPEN_OUTPUT "fopen(output, w)"
#define ERR_DO_THE_NEEDFUL "do_the_needful blew up"
#define DEFAULT_PROGNAME "george"

extern int errno;
extern char *optarg;
extern int opterr, optind;

typedef struct {
  int           verbose;
  uint32_t      flags;
  FILE         *input;
  FILE         *output;
} options_t;

int dumb_global_variable = -11;

void usage(char *progname, int opt);
int  do_the_needful(options_t *options);

int main(int argc, char *argv[]) {
    int opt;
    options_t options = { 0, 0x0, stdin, stdout };

    opterr = 0;

    while ((opt = getopt(argc, argv, OPTSTR)) != EOF)
       switch(opt) {
           case 'i':
              if (!(options.input = fopen(optarg, "r")) ){
                 perror(ERR_FOPEN_INPUT);
                 exit(EXIT_FAILURE);
                 /* NOTREACHED */
              }
              break;

           case 'o':
              if (!(options.output = fopen(optarg, "w")) ){
                 perror(ERR_FOPEN_OUTPUT);
                 exit(EXIT_FAILURE);
                 /* NOTREACHED */
              }    
              break;
             
           case 'f':
              options.flags = (uint32_t )strtoul(optarg, NULL, 16);
              break;

           case 'v':
              options.verbose += 1;
              break;

           case 'h':
           default:
              usage(basename(argv[0]), opt);
              /* NOTREACHED */
              break;
       }

    if (do_the_needful(&options) != EXIT_SUCCESS) {
       perror(ERR_DO_THE_NEEDFUL);
       exit(EXIT_FAILURE);
       /* NOTREACHED */
    }

    return EXIT_SUCCESS;
}

void usage(char *progname, int opt) {
   fprintf(stderr, USAGE_FMT, progname?progname:DEFAULT_PROGNAME);
   exit(EXIT_FAILURE);
   /* NOTREACHED */
}

int do_the_needful(options_t *options) {

   if (!options) {
     errno = EINVAL;
     return EXIT_FAILURE;
   }

   if (!options->input || !options->output) {
     errno = ENOENT;
     return EXIT_FAILURE;
   }

   /* XXX do needful stuff */

   return EXIT_SUCCESS;
}
```

## Why the main() function does not need a declaration in C/C++?
## What happens if any function in your program tries to call main()?
https://stackoverflow.com/questions/55457704/does-int-main-need-a-declaration-on-c
