# Structuring a Multi-File C Program

## Writing a Good main() Function

This is a modifed version of the original article - ["How to write a good C main function"](https://opensource.com/article/19/5/how-write-good-c-main-function). It tries to address the following:

* How to structure a C file containing a ```main()``` function that will be easy to maintain
* How to best process command line arguments

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


### 2. Defines

```C
#define OPTSTR "vi:o:f:h"
#define USAGE_FMT  "%s [-v] [-f hexflag] [-i inputfile] [-o outputfile] [-h]"
#define ERR_FOPEN_INPUT  "fopen(input, r)"
#define ERR_FOPEN_OUTPUT "fopen(output, w)"
#define ERR_DO_THE_NEEDFUL "do_the_needful blew up"
#define DEFAULT_PROGNAME "george"
```

The OPTSTR define states what command line switches the program will recommend. The getopt(3) man page describes how OPTSTR will affect getopt()'s behavior. The USAGE_FMT define is a printf()-style format string that is referenced in the usage() function. Constants - int, char, float and string contants should be defined using #defines in this part of the file. Collecting them at a single location makes it easier to fix spelling, reuse messages, and internationalize messages, if required.  

Use all capital letters when naming a #define to distinguish it from variable and function names. The define names can be a single continuous string or they can be separated with an underscore; just make sure they're all upper case.


### 2. External Declarations




The following example will be used to illustrate the ideas:

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

