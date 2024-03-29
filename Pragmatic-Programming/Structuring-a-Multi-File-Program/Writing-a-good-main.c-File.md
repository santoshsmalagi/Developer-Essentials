# Writing a Good main.c File

All C/C++ programs must contain the quintessential ``main()`` function, from a programmer's perspective this is where the execution of a program begins. However, even before the first line of ``main()`` function is executed a few other things happen in the background (this will be explained briefly in the upcoming sections). At a high level a *well-written* ``main()`` function achieves the following objectives:

* sets up the program environment
* parses and validates command line arguments (or calls a function to do so), exiting gracefully in case of incorrect usage
* initializes objects and sets up program controls based on specified arguments (or keyword-options)
* calls a sequence of functions to operate on data
* monitors return values from functions, and take further actions
* outputs the results to stdout or writes to a database/file
* post program clean up tasks - free memory, report summary, and exit gracefully
 
A good outline for a ```main.c``` to achieve the above mentioned objectives is as follows:

```C
/* main.c */
/* 1. Copyright, licensing or author information */
/* 2. Standard header includes */
/* 3. Project specific header includes */
/* 4. Defines macros and symbolic constants */
/* 5. External declarations */
/* 6. Typedefs and enums */
/* 7. Global variable declarations */
/* 8. Function prototype declarations */

int main(int argc, char *argv[]) {
/* 9. Declare variables and initialze objects required for program startup /*
/* 10. Command-line argument processing, and setup program controls */
/* 11. Function calls, error handling*/
/* 12. Output to stdout or write to file/database */
/* 13. Clean up tasks */
}

/* 14. Function definitions */ 
/* Usually in a large project functions are grouped by their purpose,
   declared in header files and defined in source files. The header
   files get included in Section - 3 above */
```

## 1. Copyright, licensing and author information
Usually this is some form of standard template text which describes copyright information, organization/author, version information, etc. It may also be helpful to briefly describe the intended purpose of this C file. Additionally it is always a good practice to add meaningful comments. Do not write about what the code is doing - instead, write about why the code is doing what it's doing

## 2.Standard header includes
The first things to add to a ```main.c``` file are includes to make a multitude of standard C library functions and variables available to the program. The ```#include``` string is a C preprocessor (cpp) directive that causes the inclusion of the referenced file, in its entirety, in the current file. At a minimum the following are recommended to be included in the ```main.c``` file:

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

## 3.Project specific header includes
Include project specific headers i.e. framework utilities, GUI enablers, or headers to provide access to API useful for the current program/project.

> **When a program is both C and C++!**  
Often times, we encounter code which is both C and C++ - i.e. C++ code, with C code declared using ```extern C``` to avoid name mangling. Such files tend to have a .C extension instead of the regular .c or .cpp extensions. For such programs, the suggested order of includes would be - (1) standard C++ headers e.g. \<iostream\>, \<fstream\>  (2) standard C headers (3) headers for C++ STL - \<vector\>, \<list\>, \<string\>, \<algorithm\> etc. (4) C++ headers for Boost if required (5) header includes for user defined libraries. An interesting article on this topic - [How to mix C and C++](https://isocpp.org/wiki/faq/mixing-c-and-cpp).

## 4. Define macros and symbolic constants

```C
#define OPTSTR "vi:o:f:h"
#define USAGE_FMT  "%s [-v] [-f hexflag] [-i inputfile] [-o outputfile] [-h]"
#define ERR_FOPEN_INPUT  "fopen(input, r)"
#define ERR_FOPEN_OUTPUT "fopen(output, w)"
#define ERR_DO_THE_NEEDFUL "do_the_needful blew up"
#define DEFAULT_PROGNAME "george"
```

Constants should be defined using the ```#define``` directive in this part of the file. Collecting them at a single location makes it easier to update and modiy them as required. The constants can include mathematical constants; string constants for ERRORs, messages which get reused; #define macros etc.  Use all capital letters when naming a ```#define``` to distinguish them from variable and function names. The ```#define``` names can be a single continuous string or they can be separated with an underscore; just make sure they're all upper case.

## 5. External declarations

An extern declaration brings that name into the namespace of the current compilation unit (aka "file") and allows the program to access that variable.

```C
extern int errno;
extern char *optarg;
extern int opterr, optind;
```

## 6. typedef's

```C++
typedef struct {
  int           verbose;
  uint32_t      flags;
  FILE         *input;
  FILE         *output;
} options_t;
```

After external declarations, declare typedef's for structures, unions, and enumerations. For naming a typedef prefer a \_t suffix to indicate that the name is a type. Since C is a whitespace-neutral programming language, whitespaces can be used to line up field names in the same column. For pointer declarations, prepend the asterisk to the name to make it clear that it's a pointer.

## 7. Global variable declarations

```C
int dumb_global_variable = -11;
```

Global variables are a bad idea and you should never use them. But if you have to use a global variable, declare them here and be sure to give them a default value. Seriously, don't use global variables.

## 8. Function prototypes

```C
void usage(char *progname, int opt);
int  do_the_needful(options_t *options);
```
A good practice (or choice of style) is to define the functions after the ```main()``` and not before. So the function prototypes need to be declared here. Early C compilers used a single-pass strategy, which meant that every symbol (variable or function name) you used in your program had to be declared before you used it. Modern compilers are nearly all multi-pass compilers that build a complete symbol table before generating code, so using function prototypes is not strictly required.

## 9-13. main()

Program execution begins at the ```main()```. The compiler does not need a forward declaration for ```main()```, because it's built into the language. The definition itself is accepted by the compiler as the declaration of ```main()```. Also program functions usually dont call the ``main`` so there is no need for a forward declaration. It cannot be overloaded or declared as ``inline`` or ``static``. 

**The linker requires that one and only one ```main()``` function exist when creating an executable program**. Dynamic-link libraries and static libraries don't have a main function. Before a program enters the ``main`` function, all static class members without explicit initializers are set to zero. Global static objects are also initialized before entry to ``main``.

``main()``  always returns a signed integer, for example it returns a 0 (zero) on success and -1 (negative one) on failure. If no return statement is provided, the compiler will provide an implicit ```return 0;``` as the last statement. In C ``main`` can also be defined as returning ``void``, though its use isn't recommended and the compiler generally produces a WARNING.  If ``main`` returns a void the ``exit()`` function can be used to return an exit value to the OS. **However, in C++ it is NOT ALLOWED to have a ``main`` as a ``void`` (or any other) type, it MUST be declared to return an ``int`` value.**

#### Why the ```main()``` function does not need a declaration?
* The compiler does not need a forward declaration for ```main()```
* In C++, since a user program never calls ```main```, so technically it never needs a declaration before the definition
* In C, a program can call ```main```, in that case it does require that a declaration be visible before the call
* Note that ```main``` does need to be known to the code that calls it. This is special code in what is typically called the C++ runtime startup code. The linker includes that code for you automatically when you are linking a C++ program with the appropriate linker options. Whatever language that code is written in, it has whatever declaration of ```main``` it needs in order to call it properly.
* Technically though, all definitions are also declarations, so your definition of main also declares main. The compiler does not need a forward declaration for ```main()```, the definiton is accepted by the compiler as the declaration of ```main()```.

#### What happens if any function in your program tries to call main()?

The C++ standard says it's undefined behavior to call ```main()``` from another function:

*" An implementation shall not predefine the main function. This function shall not be overloaded. It shall have a return type of type int, but otherwise its type is implementation defined. All implementations shall allow both of the following definitions of main … The function main shall not be used within a program. The linkage (3.5) of main is implementation-defined.*

On a hypothetical implementation, calling ```main``` could result in fun things like re-running constructors for all static variables, re-initializing the data structures used by new/delete to keep track of allocations, or other total breakage of your program. Or it might not cause any problem at all - i.e. undefined behaviour.

#### What happens before the main()?

The true entry point for most C/C++ binaries is not the ``main``, it is a function called ``_start`` which is defined in the ``libc``. After the program has been loaded into memory by the loader, it looks for the address of the ``_start`` function. The ``_start`` initializes the arguments for ``__libc_start_main()`` and calls it. ``__libc_start_main`` performs a number of important tasks such as setting up the program's runtime environment, initializing it's threading, loading custom routines prior to calling ``main()`` and registering the ``_fini()`` for cleanup post ``main()`` returns. Finally ``libc_start_main()`` calls ``main()``.  

The implementation of ``_start()`` is provided by ``libc`` and it is written in assembly. Many implementations store the ``_start`` function in a file called ``crt0.s``. Compilers typically ship with pre-compiled ``crt0.o`` object files for each supported architecture. Because ``_start`` invokes main, it also handles its return. When control returns from ``main`` to ``_start``, the next function to run is exit. The exit function calls all functions registered with ``atexitand _cxa_atexit`` during the startup process. Then ``exit`` calls the global destructors (those placed in the ``.fini``, ``.fini_array``, or ``.dtors`` sections). Finally, ``exit`` terminates the program with the return value provided by ``main``. More information - [A General Overview of What Happens Before main](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/).

#### Command line arguments
The ```main()``` function has two arguments that are traditionally called ```argc``` (argument count) and ```argv``` (argument vector), although the compiler does not require these names. For e.g. you can have ``int argi`` and ``char *args[]`` inplace of ``int argc`` and ``char *argv[]`` respectively. The types for command line arguments are defined by the language.  

> *Note that ``main`` can accept any number of command line arguments.*
 
 ``argc``, is a non-negative number corresponding to the number of elements in ``argv`` and is calculated automatically.
 
``argv`` is an array of pointers to null-terminated strings that correspond to the program arguments. 

Additional arguments known as implementation parameters can be passed to ``main``. Implementation parameters aren’t common in modern desktop environments. Traditionally, if a third parameter is passed to ``main``, that parameter is named ``envp`` and contains an array of null terminated strings representing the variables set in the user's environment. The environment block passed to ``main`` is a "frozen" copy of the current environment. Later changes to the environment won't change the values in ``envp``.
 
| Argument | Description                                                                             |
|----------|-----------------------------------------------------------------------------------------|
| argc     | Number of elements in the argument vector                                               |
| argv     | Array of character pointers corresponding to the program arguments                      |
| envp     | Array of character pointers corresponding to the environment variables and their values |

The compiler expects a ```main()``` function in one of the following forms:

```C
// no return value - C compiler will give a WARNING, NOT ALLOWED in C++
void main() { /* body */ } 
 
// no arguments, ignore any command line arguments or environmental variables
int main () { /* body */ }

// process command line arguments
int main (int argc, char *argv[]) { /* body */ }
 
// additional arguments are implementation defined e.g. array of env variables
int main (int argc, char *argv[], implementation parameters)  { /* body */ }
```

It is also possible to use the following alternative forms:
 
```C
// alternative to char *argv[], pointer form
int main (int argc, char **argv) { /* body */ }
 
// some implementations may require argv to be of const type
int main (int argc, const char **argv) { /* body */ }                   
```
 
For example, the following provides a list of the environment variables at the time the function is called:

```C
// envp provides a list of environment variables and their values
int main (int argc, char *argv[], char *envp[]) { /* body */ }         
```

Windows Visual C++ programming environment also supports a wide-character version of the main function.
 
```C
// Visual studio also supports a wchar version of parameters
int wmain(int argc, wchar_t *argv[], wchar_t *envp[]) { /* body */ }
 ```

## 14. Function definitions

Finally, write functions that aren't boilerplate. Functions should almost always validate their input in some way. If full validation is expensive, try to do it once and treat the validated data as immutable. It also makes sense to group functions by their purpose and define them in a seperate source file, while including the header in which they are declared.

A good practice is that - if something goes wrong in the middle of a function, it's a good time to return an error condition. Writing a ton of nested if statements to just have one return is never a "good idea. Finally, if you write a function that takes four or more arguments, consider bundling them in a structure and passing a pointer to the structure. This makes the function signatures simpler, making them easier to remember and not screw up when they're called later. It also makes calling the function slightly faster, since fewer things need to be copied into the function's stack frame.  

The big class of errors to avoid is de-referencing a NULL pointer. This will cause the operating system to send a special signal called SYSSEGV, which results in unavoidable death. The last thing users want to see is a crash due to SYSSEGV. It's much better to catch a NULL pointer in order to emit better error messages and shut down the program gracefully.

## Complete example main.c file

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

The guts of this ```main()``` function is a while loop that steps through argv looking for command line options and their arguments (if any). When a known command line option is detected, option-specific behavior happens. Some options have an argument, when an option has an argument, the next string in ```argv``` is available to the program. Files are opened for reading and writing or command line arguments are converted from a string to an integer value.

## Reference

["How to write a good C main function"](https://opensource.com/article/19/5/how-write-good-c-main-function).  
