 # Extern and Static Variables in a Multi-File Program
 
The following example illustrates the concept of multi-file C programming by referencing functions defined in external files using the ```extern``` keyword. 
 
 * ```main.c```          # processes command line arguments, references functions declared externally in other files
 * ```standard.c```      # function definitions for add(), sub(), mul(), divide()
 * ```advanced.c```      # function definitions for mod(), power()

A prototype for a function can be put in the file of code which calls the function, typically at/near the top of the file. Alternatively, the prototype can be put in a header file and an include statement for this header file inserted into the code file.  The challenge with using ```extern``` declarations is that:

* the order of prototype declarations matter - i.e. a function needs to be present in the current file scope before it can be called by any other function
* if the external function operates on data structures, variables or constants which are restricted to the file in which it is defined (i.e. static) then calling the external function would throw an error when it tries to access these data items

To compile:

```bash
$ cc -c stadard.c
$ cc -c advanced.c
$ cc -Wall -o calc main.c standard.o advanced.o -lm

or

$ cc -Wall -o calc main.c standard.c advanced.c -lm
```

Note the use of `-lm` flag during compilation required to successfully link to the math library. To execute:

```bash
$ ./calc 2 + 1
$ ./calc 1 '*' 2      # note the use of '*', not using single quotes results in wild card expansion 
                      # casuing the argc != 4 condition check to fail  
```

Another way to get avoid wildcard expansion during command execution is to:

```bash
$ set -f              # disable the glob (noglob)
$ set -o noglob       # another method to disable glob
$ set +o noglob       # undo glob by
```

```C
//-----------------------------------------------------
// main.c
//-----------------------------------------------------

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

extern double add(double, double);
extern double sub(double,double);
extern double mul(double, double);
extern double divide(double, double);
extern int mod(int, int);
extern double power(double, double);

int main(int argc, char *argv[])
{

  if (argc != 4)
  {
    printf("ERROR - specify one of the following:\n"
           "calc <num1> '+' <num2>\n"
           "calc <num1> '-' <num2>\n"
           "calc <num1> '/' <num2>\n"
           "calc <num1> '%%' <num2>\n"
           "calc <num1> 'p' <num2>\n"
           "Also, on your terminal do - 'set -f'\n");
    
    return -1;
  }

  double num1 = atof(argv[1]);
  double num2 = atof(argv[3]);

  if (strcmp(argv[2],"+") == 0)
    printf("%f + %f = %f\n", num1, num2, add(num1, num2));

  else if (strcmp(argv[2],"-") == 0)
    printf("%f - %f = %f\n", num1, num2, sub(num1, num2));

  else if (strcmp(argv[2],"*") == 0)
    printf("%f * %f = %f\n", num1, num2, mul(num1, num2));

  else if (strcmp(argv[2],"/") == 0)
  {
    if (num2 == 0)
    {
      printf("ERROR - DIV by 0\n");
      return -1;
    }
    
    printf("%f / %f = %f\n", num1, num2, divide(num1, num2));
  }

  else if (strcmp(argv[2],"%") == 0)
  {
    if ( (int)num2 == 0)
    {
      printf("ERROR - DIV by 0\n");
      return -1;
    }
    printf("%d %% %d = %d\n", (int)num1, (int)num2, mod((int)num1, (int)num2));
  }

  else if (strcmp(argv[2],"p") == 0)
  {
    if (num2 < 0)
    {
      printf("ERROR - exponent index cannot be negative\n");
      return -1;
    }
    printf("%f ^ %f = %f\n", num1, num2, power(num1, num2));
  }

  else
  {
    printf("ERROR - specify one of the following:\n"
                    "calc <num1> '+' <num2>\n"
                    "calc <num1> '-' <num2>\n"
                    "calc <num1> '/' <num2>\n"
                    "calc <num1> '%%' <num2>\n"
                    "calc <num1> 'p' <num2>\n"
                    "Also, on your terminal do - 'set -f'\n");
    return -1;
  }

  return 0;
}

```

```C
// ----------------------------------------------------
// standard.c
// ----------------------------------------------------
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
  return (num1 * num2);
}

double divide(double num1, double num2)
{
  return (num1 / num2);
}
```

```C
//---------------------------------------------------
//advanced.c
//---------------------------------------------------
#include <stdio.h>
#include <math.h>

static int mod(int num1, int num2)
{
  return ((int)num1 % (int)num2);
}

double power(double num1, double num2)
{
  return pow(num1, num2);
}
```
