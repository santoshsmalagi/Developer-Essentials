# Doxygen Template for C++ Programs
This is a collection of templates to generate documentation for C++ source code using Doxygen. The content of the templates has been inspired from multiple sources:
* *The Elements of C++ Style* by Trevor Misffeldt, Gregory Bumgardner, and Andrew Gray
* Steve Oualline's classic book - *C Elements of Style*
* Examples provided in Doxygen Manual

*The templates use the Qt syle - //! as the preffered style for generating C++ documentation using Doxygen. For more styles see - [Comment Blocks](/Doxygen/02.Comment-Blocks.md)*.

## 1. Ignoring comments

For comments which need not be processed by Doxygen use a normal C++ ``//`` comment:

```C++
////////////////////////////////////////////////////////////////////////////////
// Copyright (C) 1988-2021 Free Software Foundation, Inc.                     //
// This file is part of GCC.                                                  //
// GCC is free software; you can redistribute it and/or modify it under       //
// the terms of the GNU General Public License as published by the Free       //
// Software Foundation; either version 3, or (at your option) any later       //
// version.                                                                   //
////////////////////////////////////////////////////////////////////////////////
```

> **IMPORTANT** - *in your Doxyfile configuration ALWAYS set ``JAVADOC_BANNER = NO``, such that Doxygen will interpret these boxed comments as comments and ignore them.*

## 2. Documenting files
All files must have file comments. File comments include details like licensing and copyright if any, author information, the purpose of the file, known bugs, change log etc. If the file defines the ``main()``, then it is also helpful to include details like - program usage, warnings/special instructions for someone building the program, a summary of the most important data structures or algorithms, references if any, and finally any other miscelleanous notes.  

```C++
//!//////////////////////////////////////////////////////////////////////////////
//! @file main.c
//! @brief Process arguments and main application controller
//! This program processes the command line arguments and stores them in the
//! appropriate data structures. If any error is encountered during execution
//! it displays a suitable error message and terminates the program.
//!///////////////////////////////////////////////////////////////////////////////
 ```
  
 > *All files must have file comments, and must include @file command, otherwise global entities - functions, typedefs, enums, macros and variables will not be documented, even though Doxygen compatible comments are used.*

## 3. Documenting classes

Class documentation involves use of block comments to describe the overall purpose of the class, and using inline comments to describe the class members variables and methods. Though it may not always be necessary to document the obivious - e.g. the purpose of a ctor or a dtor.

```C++
//!  @brief A test class. 
//!  A more elaborate class description.
 
class QTstyle_Test
{
  public:
 
    //! An enum.
    //! More detailed enum description.
    enum TEnum { 
                 TVal1, //!< Enum value TVal1.  
                 TVal2, //!< Enum value TVal2.  
                 TVal3  //!< Enum value TVal3.  
               }
               //! and enum variable
               *enumPtr,
               //! an enum pointer
               enumVar;  
    
    //! A constructor.
    //! A more elaborate description of the constructor.
    QTstyle_Test();
 
    //! A destructor.
    //! A more elaborate description of the destructor.
   ~QTstyle_Test();
    
    //! @brief A normal member taking two arguments and returning an integer value.
    //! @param a an integer argument.
    //! @param s a constant character pointer.
    //! @return The test results
    //! @see QTstyle_Test(), ~QTstyle_Test(), testMeToo() and publicVar()
    int testMe(int a,const char *s);
       
    //! A pure virtual member.
    //! @see testMe()
    //! @param c1 the first argument
    //! @param param c2 the second argument
    virtual void testMeToo(char c1,char c2) = 0;
   
    int publicVar;  //!< A public variable.
       
    //! @brief A function variable.
    int (*handler)(int a,int b);
};
```
It may also be possible to be creative and re-write the above example using multiple commenting styles, as shown in the [Doxygen Reference](https://www.doxygen.nl/manual/docblocks.html):

```C++
//!  A test class. 
/*!
  A more elaborate class description.
*/
 
class QTstyle_Test
{
  public:
 
    //! An enum.
    /*! More detailed enum description. */
    enum TEnum { 
                 TVal1, /*!< Enum value TVal1. */  
                 TVal2, /*!< Enum value TVal2. */  
                 TVal3  /*!< Enum value TVal3. */  
               } 
         //! Enum pointer.
         /*! Details. */
         *enumPtr, 
         //! Enum variable.
         /*! Details. */
         enumVar;  
    
    //! A constructor.
    /*!
      A more elaborate description of the constructor.
    */
    QTstyle_Test();
 
    //! A destructor.
    /*!
      A more elaborate description of the destructor.
    */
   ~QTstyle_Test();
    
    //! A normal member taking two arguments and returning an integer value.
    /*!
      \param a an integer argument.
      \param s a constant character pointer.
      \return The test results
      \sa QTstyle_Test(), ~QTstyle_Test(), testMeToo() and publicVar()
    */
    int testMe(int a,const char *s);
       
    //! A pure virtual member.
    /*!
      \sa testMe()
      \param c1 the first argument.
      \param c2 the second argument.
    */
    virtual void testMeToo(char c1,char c2) = 0;
   
    //! A public variable.
    /*!
      Details.
    */
    int publicVar;
       
    //! A function variable.
    /*!
      Details.
    */
    int (*handler)(int a,int b);
};
```

## 4. Documenting global/static functions
Functions can be documentated at thier declaration (i.e. in header files) or at their definition (in the source file). For functions which have been declared and defined (e.g. ``static`` functions) in the same file it may be much cleaner to document them at thier definition. See - [Elements of C++ Style](https://github.com/santoshsmalagi/CPP/blob/master/Part%20I%20-%20The%20Basics/The-Elements-of-C++-Stlye.md) for how to document functions.

A function's documentation must include the following fields:
* A brief one line description about the function and its purpose specified using ``@brief``
* Detailed description about the function (as explained in 
* Parameters (specified using ``@param``)
    * a list of parameters (one per line) and a brief description of each of them
* Return value (specified using ``@return``)
    * describes what value the function returns
* any other useful information such as:
    * ``@see`` to refer to related functions
    * ``@note`` to add additional information, remarks, notes etc.
    * ``@date`` to describe when the source file was created
    * ``@author`` to describe who the author is //! 
    * ``@warning`` to warn the user about any special restrictions regarding function usage 

```C
//!/////////////////////////////////////////////////////////////////////////////
//! @brief accepts three unsigned numbers and returns true if they form\
//!        a pythagorean triplet
//! This function accepts three unsigned numbers, calculates the
//! square of each, and checks to see if sum of squares of the the two 
//! smaller numbers equals the square of the third. 
//! @param num1 the first number
//! @param num2 the second number
//! @param num3 the third number
//! @return returns true if the numbers form a pythagorean triplet, otherwise returns false
//! @see square
//! @note a^2 = b^2 + c^2
//!/////////////////////////////////////////////////////////////////////////////
 int isPythagoreanTriplet(unsigned a, unsigned b, unsigned c);
```

## 5. Documenting global, static variables

To document global or static variables, put an additional ```<``` marker in the comment block. This is more of an inline comment.

```C
int var;                   ///< stores the number of variables or
int var;                   //!< stores the number of variables
```

## 6. Documenting typedefs
A ``typedef`` can be documented with an inline comment or block comment, though inline comments are more preferable.

```C
typedef unsigned int u_int32;               //!< a type definition for unsigned 32 bit integer
```
## 7. Documenting unions, bit fields and enumerated constants
Unions, bit fields, and enumerated types, are documented just like structs - a combination of descriptive block comments describing the structure/union or bit field, and inline comments for the members.

## 8. Documenting macros
A macro is documented using block comments.

```C
/*!*****************************************************************************
 * @brief A macro that returns the maximum of a and b.
 *******************************************************************************/
#define MAX(a,b) (((a)>(b))?(a):(b))
```

> *OK, enough writing about writing code; the code itself is much more interesting. Have fun -:)*
