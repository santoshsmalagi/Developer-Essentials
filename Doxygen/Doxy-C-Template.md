# Doxygen Template for C Programs
This is a collection of readily usable templates to generate documentation for C source code using the Doxygen system.  

Code documentation is absolutely vital to keep the codebase readable and to preserve valuable knowledge. While comments and code documentation are very important, the best code is self-documenting. When documenting code do it with the audience in mind - the next developer who will need to understand the code to fix bugs and ship enhancements. *Be generous — the next one may be you!*  

The following templates are mostly inspired from Steve Oualine's classic - *C Elements of Style* and *The Elements of C Programming Style* by Jay Ranade and Alan Nash.

> *The templates use the Qt syle - /\*! as the preffered style for generating documentation using Doxygen. The \* always ends on the 80th column and \/ on the 81st!*

## 1. Comments which should not be processed by Doxygen

For comments which need not be processed by Doxygen e.g. copyright notice use the boxed comment style:

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

**IMPORTANT** - in your Doxyfile configuration ALWAYS set ``JAVADOC_BANNER = NO``, such that Doxygen will always interpret these boxed comments as *comments* and ignore them.

## 2. File comments
After the licence and copyright stuff, author information etc. include a heading comment at the beginning of each file, that explains the purpose of the file.  

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
