## 1. Philosophy behind design of multi-file C Programs

Large C programs can be constructed and laid out efficiently as a collection of multiple source files (.c, .cpp or .C) and corresponding header files (.h). This provides several advantages:

* **Readability and ease of maintainence**
	* makes the program convenient enough to edit and modify
	* common utilities such as frameowrk services, reporting functions etc. can be shared  - there by avoiding code duplicacy
* **Each of the modules can be developed and tested seperately**
	* files can be compiled separately into \*.o files, later all the \*.o files can be linked together to create a binary executable
* **Promotes the concept of abstraction**
	* exposes the Application Programming Interface (API) for use by functions external to a file 
	* each file can have access to a set of names that are private to that file, and to other names that are shared across all the files of the complete program

## 2. Layout of a multi-file C project

Though there is no "one right way" to divide a large program (or a project), in general one source file defines the ```main()``` while others contain function definitions. Corresponding header files declare these functions and any associated data structures, variables or constants. Global data structures and constants may be defined in a ```main.h```. In order for functions in a file *sourceFile1.c* to call a function defined in another file *sourceFile2.c*, the corresponding header file *sourceFile2.h* must be included in *sourceFile1.c*. 

* **Organize your program as a collection of multiple files, each file containing similar functions grouped together:**
	* start by laying out a directory structure consisting of multiple empty files
		* every source file (.c, .cpp or .C) should ideally be accompanied by a header file (.h)
		* include a Makefile with recipes to build and compile the program
	* minimize the number of functions in a file, making it easy to maintain and read

* **All functions operating on similar objects or data structures must be kept in a single source file**
	* re-usability of the data structure and objects of this type
	* all related functions are stored together
	* modifications or changes to the object require only one file to be modified

* **Declare once and re-use multiple times**
	* main purpose of header files is to make definitions and declarations accessible to functions in more than one file
	* if a data structure/variable or function is supposed to be local to a file, then it must not be included in the header file 
	* every source file which uses definitions from *foo.h* must contain an "include" statement for *foo.h*, this directs the compiler to read the code in foo.h (i.e. function definitions from *foo.c*) when it compiles the source file

* **Always include 'Header Gaurds' in header files.**
* **Header files must never include 'function definitions', only 'function declarataions'**
* **Header files are not fed to the compiler unless there is a need to use Pre-Compiled headers**
	* Header files are intended to be included into implementation files, not fed to the compiler as independent translation units
	* To make builds faster, GCC allows you to precompile a header file - [Pre-Compiled Headers](https://gcc.gnu.org/onlinedocs/gcc/Precompiled-Headers.html)
	* These can include system headers which are not going to change, or header files in very large C++ projects

## 3. A note on header Files

If a piece of source file is named *sourceFile.c*, the corresponding header file is traditionally named *sourceFile.h*. The main purpose of header files is to make definitions and declarations accessible to functions in more than one files. A header file typically contains:

* definitions of global variables and global constants
* definitions of classes, structs, unions, enumerated types 
* typedef statements used to create type names
* declarations of functions, called "prototypes"
* include statements for other files, including C/C++ library files (e.g. iostream.h, math.h)
* comments associated with all of the above 

**In general, header files should not contain function definitions, only function decrlarations. Exceptions involve very short functions, so short that they can be included in a class definition or declared as inline functions. These functions do trivial things such as increment a counter, return the larger of two numbers, or return the value of a private data member for a class.  If the function does something non-trivial, it belongs in a source file.**  

> #include<filename.h> - includes standard library header files, searched for in a set of pre-determined system directories <br>
  #include "filename"  - to include programmer-defined header files, files are searched first in the current directory <br>
                         then in the system directories

### 4. Header Gaurds

The ```#ifdef```, ```#define```, ```#endif``` construction is collectively known as a "header guard." 

```C

// -----------------------------------------------
// header.h
// -----------------------------------------------

#ifndef _HEADER_H
#define _HEADER_H

// header file contents 

#endif
```

This keeps the C compiler from including a header file more than once per file. In case of a multi file C project it is common for header files to include other header files.  If this is done extensively, it could lead to a situation in which the same header declarations are included multiple times.  This is probably OK for function prototypes, and for variable declarations, but it produces illegal C programs if enums, structs, or unions are repeated, since the member names and enum constants defined by these declarations become multiply defined.  A header gaurd is therefore an **ABSOLUTE MUST** when creating header files.

Consider the following example:

```C

// --------------------------------------------------
// grandparent.h
// --------------------------------------------------

struct foo {
    int member;
};

// --------------------------------------------------
// parent.h
// --------------------------------------------------

#include "grandparent.h"

// --------------------------------------------------
// child.h
// --------------------------------------------------

#include "grandparent.h"
#include "parent.h"

// --------------------------------------------------
// child.c
// --------------------------------------------------

#include "child.h"

```

The net result is that the file "child.c" has indirectly included two copies of ```struct foo``` resulting in a compilation error, since the structure type ```foo``` will thus be defined twice. This problem can be fixed by including a header gaurd for ```grandparent.h```.

```C

// --------------------------------------------------
// grandparent.h
// --------------------------------------------------

#ifndef _GRANDPARENT_H
#define _GRANDPARENT_H

struct foo {
    int member;
};

#endif
```

*A project using header guards must work out a coherent naming scheme for its include guards, and make sure its scheme doesn't conflict with that of any third-party headers it uses, or with the names of any globally visible macros. For this reason, most C and C++ implementations provide a non-standard* ```#pragma once``` *directive. This directive, inserted at the top of a header file, will ensure that the file is included only once.*

### Computed Includes

Sometimes a decision has to be made regarding which header file to inlcude depending on - the compiler flags or conditional parameters, architecture of the target machine etc. In such a situation the ```#if``` pre-processor construct can be used as follows:

```C
#if SYSTEM_1
   # include "system_1.h"
#elif SYSTEM_2
   # include "system_2.h"
#elif SYSTEM_3
```
## 5. Compiling a multi-file C project

In general, to compile a program consisting of multiple files, start by compiling each of the 'non-main' source files with a *-c* flag to generate an object file for each of them. The *-c* flag tells the compiler to produce an object file ```(\*.o)``` that can be linked together with other compiled object files.  It also prevents the compiler from complaining about the lack of a ```main()``` function and about use of functions that are not defined (i.e. because they are defined in other code files). The next step is to compile the ```main.c``` file linking all the object files produced to create the binary executable.

The following is one such example:

```bash
$ cc -c stadard.c
$ cc -c advanced.c
$ cc -Wall -o calc main.c standard.o advanced.o -lm

or

$ cc -Wall -o calc main.c standard.c advanced.c -lm
```

Note the use of `-lm` flag during compilation required to successfully link to the math library.

## 6. Simple illustrative example of a multi-file C project

Let us try to illustrate the above ideas by building a simple calculator application which:
* accepts two real numbers and an operator (+, -, \*, %, p) as command line arguments
* validates command line arguments and calls appropriate functions
* if an invalid input is entered, exits gracefully with an appropriate ERROR message
  
| File       	| Remarks                                          	|
|------------	|--------------------------------------------------	|
| main.c     	| overall command line argument processing and function calls      	|
| main.h     	| global defines and macros                        	|
| standard.c 	| definitions for add(), sub(), mul() and divide() 	|
| standard.h 	| declarations for the above functions             	|
| advanced.c 	| definitions for mod() and power()                	|
| advanced.h 	| declarations for mod() and power()               	|
| Makefile   	| recipes for building and compiling our program   	|


```Makefile

//###############################################################
// Makefile
//###############################################################

all: main.o standard.o advanced.o
		cc -Wall -o calc main.o standard.o advanced.o -lm

main.o: main.c
		cc -Wall -c main.c

standard.o: standard.c
		cc -Wall -c standard.c

advanced.o: advanced.c
		cc -Wall -c advanced.c

clean:
		rm calc main.o standard.o advanced.o
```

```C

//--------------------------------------------------------------
// main.c
//--------------------------------------------------------------

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "main.h"
#include "standard.h"
#include "advanced.h"

int main(int argc, char *argv[])
{
  if (argc != 4)
  {
    ARG_ERROR
    return -1;
  }

  double num1 = atof(argv[1]);
  double num2 = atof(argv[3]);

  if (strcmp(argv[2], "+") == 0)
    printf("%f + %f = %f\n", num1, num2, add(num1, num2));

  else if (strcmp(argv[2], "-") == 0)
    printf("%f - %f = %f\n", num1, num2, sub(num1, num2));

  else if (strcmp(argv[2], "*") == 0)
    printf("%f * %f = %f\n", num1, num2, mul(num1, num2));

  else if (strcmp(argv[2], "/") == 0)
  {
    if (num2 == 0)
    {
      DIV_ZERO_ERROR
      return -1;
    }
    printf("%f / %f = %f\n", num1, num2, divide(num1, num2));
  }

  else if (strcmp(argv[2], "m") == 0)
  {
    if (num2 == 0)
    {
      DIV_ZERO_ERROR
      return -1;
    }
    printf("%d %% %d = %d\n", (int)num1, (int)num2, mod(num1, num2));
  }

  else if (strcmp(argv[2], "p") == 0)
  {
    if (num2 == 0)
    {
      NEG_EXP_ERROR
      return -1;
    }
    printf("%f ^ %f = %f\n", num1, num2, power(num1, num2));
  }

  else
  {
    ARG_ERROR
    return -1;
  }
  
  return 0;
}
```

```C

//-----------------------------------------------------------------------
// main.h
//-----------------------------------------------------------------------

#ifndef _MAIN_H
#define _MAIN_H

#define ARG_ERROR  printf("ERROR - SPECIFY:\n" \
                          "calc <num1> '+' <num2>\n" \
                          "calc <num1> '-' <num2>\n" \
                          "calc <num1> '/' <num2>\n" \
                          "calc <num1> '%%' <num2>\n" \
                          "calc <num1> 'p' <num2>\n" \
                          "Also, on your terminal do - 'set -f'\n");

#define DIV_ZERO_ERROR printf("ERROR - DIV BY ZERO\n");
#define NEG_EXP_ERROR printf("ERROR - EXPONENT CANNOT BE NEGATIVE\n");

#endif

```

```C

//----------------------------------------------------------------------
// standard.c
//----------------------------------------------------------------------

#include <stdio.h>
#include <stdlib.h>

double add(double num1, double num2)
{
  return (num1 + num2);
}

double sub(double num1, double num2)
{
  return (num1 - num2);
}

double mul(double num1, double num2)
{
  return (num1*num2);
}

double divide(double num1, double num2)
{
  return (num1/num2);
}

```

```C

//-----------------------------------------------------------------------
// standard.h
//-----------------------------------------------------------------------

#ifndef _STANDARD_H
#define _STANDARD_H

double add(double num1, double num2);
double sub(double num1, double num2);
double mul(double num1, double num2);
double divide(double num1, double num2);

#endif

```

```C

// ----------------------------------------------------------------------
// advanced.c
// ----------------------------------------------------------------------

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int mod(int num1, int num2)
{
  return (num1 % num2);
}

double power(double num1, double num2)
{
  return (pow(num1,num2));
}

```

```C

// -----------------------------------------------------------------------
// advanced.h
// -----------------------------------------------------------------------

#ifndef _ADVANCED_H
#define _ADVANCED_H

int mod(int num1, int num2);
double power(double num1, double num2);

#endif

```
## 7. Summary
Writing a multi-file program in C requires a little more planning on behalf of the programmer than just a single main.c. But just a little effort up front can save a lot of time and headache when you refactor as you add functionality.

To recap, I like to have a lot of files with a few short functions in them. I like to expose a small subset of the functions in those files via header files. I like to keep my constants in header files, both numeric and string constants. I love Makefiles and use them instead of Bash scripts to automate all sorts of things. I like my main() function to handle command-line argument parsing and act as a scaffold for the primary functionality of the program.
