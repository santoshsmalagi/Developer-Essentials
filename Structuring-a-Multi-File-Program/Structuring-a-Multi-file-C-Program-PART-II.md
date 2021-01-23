## Philosophy behind design of multi-file C Programs

Large C programs can be constructed and laid out efficiently as a collection of multiple source file (.c, .cpp or .C) and corresponding header files (.h). This provides several advantages:

* Each of the modules can be developed and tested seperately
	* files can be compiled separately, into a \*.o file, later, all the \*.o files can be linked together to create a binary executable
* Promotes the concept of abstraction
	* only exposes the Application Programming Interface (API) for use by functions external to a file 
	* each file can have access to a set of names that are private to that file, and to other names that are shared across all the files of the complete program

## Layout of a multi-file C project

Though there is no hard and fast rule to organize a multi-file C program (or a project) the following are some of the suggested best practices:

* **Organize your program as a collection of multiple files, each file containing similar functions grouped together:**
	* start by laying out a directory structure consisting of multiple empty files
		* every source file (.c, .cpp or .C) should ideally be accompanied by a header file (.h)
		* include a Makefile with recipes to build and compile the program
	* minimize the number of functions in a file, making it easy to maintain and read

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


## A note on header Files

* Header guards
	* header gaurds are an ABSOLUTE NECESSITY to avoid multiple inclusions of a single header file in a multi-file C project

## Extern and static variables
## Compiling a multi-file C project
## Summary
Writing a multi-file program in C requires a little more planning on behalf of the programmer than just a single main.c. But just a little effort up front can save a lot of time and headache when you refactor as you add functionality.

To recap, I like to have a lot of files with a few short functions in them. I like to expose a small subset of the functions in those files via header files. I like to keep my constants in header files, both numeric and string constants. I love Makefiles and use them instead of Bash scripts to automate all sorts of things. I like my main() function to handle command-line argument parsing and act as a scaffold for the primary functionality of the program.

## Simple illustrative example of a multi-file C project

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
