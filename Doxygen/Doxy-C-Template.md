# Doxygen Template for C Programs
This is a collection of templates to generate documentation for C source code using Doxygen. The content of the templates has been inspired from multiple sources:
* Steve Oualline's classic book - *C Elements of Style*
* Examples provided in Doxygen Manual

*The templates use the Qt syle - /\*! as the preffered style for generating documentation using Doxygen. The \* always ends on the 80th column and \/ on the 81st! For more styles see - [Comment Blocks](/Doxygen/02.Comment-Blocks.md)*.

## 1. Ignoring comments

For comments which need not be processed by Doxygen use the boxed comment style:

```C
/******************************************************************************
* Copyright (C) 1988-2021 Free Software Foundation, Inc.                      *
* This file is part of GCC.                                                   *
* GCC is free software; you can redistribute it and/or modify it under        *
* the terms of the GNU General Public License as published by the Free        *
* Software Foundation; either version 3, or (at your option) any later        *
* version.                                                                    *
*******************************************************************************/
```

> **IMPORTANT** - *in your Doxyfile configuration ALWAYS set ``JAVADOC_BANNER = NO``, such that Doxygen will interpret these boxed comments as comments and ignore them.*

## 2. Documenting files
All files must have file comments. File comments must necessarily include license and copyright stuff, author information etc. If you do not wish to process liecensing and copyright information through Doxygen, it can be ignored as mentioned above. Always include a heading comment at the beginning of each file, that explains the purpose of the file.  

If the file defines the ``main()``, then the top of the file must also briefly describe the program, its usage, any warnings/special instructions for someone building the program, describe in short the most important data structures or algorithms, references if any, and finally add any other miscelleanous remarks.  

```C
/*!****************************************************************************
 * @file main.c
 * @brief Process arguments and main application controller
 * This program processes the command line arguments and stores them in the
 * appropriate data structures. If any error is encountered during execution
 * it displays a suitable error message and terminates the program.
 *******************************************************************************/
 ```
To document a global C function, typedef, enum, or pre-processor definition you must first document the file that contains it (usually this will be a header file, because that file contains the information that is exported to other source files).
 
 > *All files must have file comments, and must include @file command, otherwise global entities - functions, typedefs, enums, macros and variables will not be documented, even though Doxygen compatible comments are used.*

The following settings in the Doxyfile determine which entities from the file are documented:

```Shell
# all entities except ``static`` functions/variables and ``private`` class members are documented
# default = NO
EXTRACT_ALL = YES      

# if EXTRACT_PRIVATE tag is set to YES, all private members of a class will be documented
# default = NO
EXTRACT_PRIVATE = YES   

# if EXTRACT_PRIV_VIRTUAL tag is set to YES, private virtual methods of a class are included
# default = NO
EXTRACT_PRIV_VIRTUAL = YES 

# If the EXTRACT_PACKAGE tag is set to YES, all members with package or internal included
# default = NO.
EXTRACT_PACKAGE = YES

# if EXTRACT_STATIC tag is set to YES, all static members of a file will be documented
# default = NO
EXTRACT_STATIC = YES   

# If the EXTRACT_LOCAL_CLASSES tag is set to YES, classes (and structs) defined
# locally in source files will be included in the documentation. If set to NO,
# only classes defined in header files are included.
# default = YES.
EXTRACT_LOCAL_CLASSES  = YES     
```

## 3. Documenting global/static functions
Declaration comments describe use of the function (when it is non-obvious); comments at the definition of a function describe operation. Do not repeat the same information in multiple locations. Function comments may be ommitted if the function is simple and obvious.

> *For functions which have been declared and defined (e.g. ``static`` functions) in the same file it may be much cleaner to document them at the definition.*

A function's documentation must include the following fields:
* A brief one line description about the function and its purpose specified using ``@brief``
* Detailed description about the function which describe what the function does and how to use it
    * If there is anything tricky about how a function does its job, the function definition MUST have an explanatory comment regarding this 
    * For class member functions whether the object remembers reference arguments beyond the duration of the method call, and whether it will free them or not.
    * If the function allocates memory that the caller must free
    * Whether any of the arguments can be a null pointer
    * If there are any performance implications of how a function is used
    * If the function is re-entrant, what are its synchronization assumptions?
* Parameters (specified using ``@param``)
    * a list of parameters (one per line) and a brief description of each of them
* Return value (specified using ``@return``)
    * describes what value the function returns
* in addition to these, any other useful information can always be added
    * use ``@see`` to refer to related functions
    * use ``@note`` to add additional information, remarks, notes etc.
    * use ``@warning`` to warn the user about any special restrictions regarding the function usage etc. 
 
```C
/*!****************************************************************************
 * @brief accepts three unsigned numbers and returns true if they form\
          a pythagorean triplet
 * This function accepts three unsigned numbers, calculates the
 * square of each, and checks to see if sum of squares of the the two 
 * smaller numbers equals the square of the third. 
 * @param num1 the first number
 * @param num2 the second number
 * @param num3 the third number
 * @return returns true if the numbers form a pythagorean triplet, otherwise returns false
 * @see square
 * @note a^2 = b^2 + c^2
 *******************************************************************************/
 int isPythagoreanTriplet(unsigned a, unsigned b, unsigned c);
```

## 4. Documenting global, static variables

To document global or static variables, put an additional ```<``` marker in the comment block. This is more of an inline comment.

```C
int var;                   /*!< stores the number of variables */ or
int var;                   //!< stores the number of variables
```

## 5. Documenting typedefs
A ``typedef`` can be documented with an inline comment or block comment, though inline comments ae more preferable.

```C
typedef unsigned int u_int32;               /*!< a type definition for unsigned 32 bit integer */
```

## 6. Documenting enumerated constants

Describe the ``enum`` type in detail, why are you defining an enumerated type in the first place, what are the legal and illegal values, and how will they be used. Documenting enumerated types involves a combination of block comments to descibe the structure overall, but using inline comments to describe the constants themselves in greater detail.

```C
/*!*****************************************************************************
 * @brief an enumerated type
 * more detailed enum description.
 *******************************************************************************/
 enum TEnum 
 {
     TVal1,               /*!< enum value TVal1. */
     TVal2,               /*!< enum value TVal2. */
     TVal3                /*!< enum value TVal3. */  
  }
  *enumPtr,               /*!< enum pointer.     */
   enumVar;               /*!< enum variable     */
```

## 7. Documenting structures, unions and bit fields
Structs, unions and bit fields are documented just like enumerated types - a combination of descriptive block comment describing the structure/union or bit field, and inline comments for the members.

## 8. Documenting macros
A macro is documented using block comments.

```C
/*!*****************************************************************************
 * @brief A macro that returns the maximum of a and b.
 *******************************************************************************/
#define MAX(a,b) (((a)>(b))?(a):(b))
```

> *OK, enough writing about writing code; the code itself is much more interesting. Have fun -:)*
