## Philosophy behind design of multi-file C Programs
## Layout of a multi-file C project
## A note on header Files
## Extern and static variables
## Compiling a multi-file C project
## Simple illustrative example of a multi-file C project

```Makefile
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

//---------------------------------------------------
// main.c
//---------------------------------------------------

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

//------------------------------------------------
// main.h
//------------------------------------------------

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

//---------------------------------------------------
// standard.c
//---------------------------------------------------

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

//----------------------------------------------------
// standard.h
//----------------------------------------------------

#ifndef _STANDARD_H
#define _STANDARD_H

double add(double num1, double num2);
double sub(double num1, double num2);
double mul(double num1, double num2);
double divide(double num1, double num2);

#endif

```

```C

// ----------------------------------------------------
// advanced.c
// ----------------------------------------------------

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

// ----------------------------------------------------------
// advanced.h
// ----------------------------------------------------------

#ifndef _ADVANCED_H
#define _ADVANCED_H

int mod(int num1, int num2);
double power(double num1, double num2);

#endif

```
