# GCC - the Essentials

# What is the gcc?

GCC stands for “GNU Compiler Collection”. GCC is an integrated distribution of compilers for several major programming languages. These languages currently include C, C++,
Objective-C, Objective-C++, Fortran, Ada, D, Go, and BRIG (HSAIL).  

> When we talk about compiling one of those languages, we might refer to that compiler by its own name, or as GCC. Either is correct.

The language-independent component of GCC includes the majority of the optimizers, as well as the “back ends” that generate machine code for various processors.  

The part of a compiler that is specific to a particular language is called the “front end”.

After you edit your source code with a text-editor, the next step is to to compile your program using gcc.  

Most of the compilers for languages other than C have their own names. The C++ compiler is G++, the Ada compiler is GNAT, and so on.

Historically, compilers for many languages, including C++ and Fortran, have been implemented
as “preprocessors” which emit another high level language such as C. None of
the compilers included in GCC are implemented this way; they all generate machine code
directly.

```Shell
g++ -std=<std> -o <executable> <src1.cpp> [src2.cpp ...]              # to compile witha specific version of C++
```

In order to debug an executable, it must be compiled with –g flag to save the symbol table. The symbol table contains the list of memory addresses corresponding to the variables and various machine instructions.

```Shell
gcc -Wall –g -o <executable_name>  <source files>
gcc –Wall –Og –o <executable_name> <source files>
gcc –Wall –std=C++17 –Og –o <executable_name> <source files>
```

Run the program as follows:

```Shell
./<executable>
```

https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html  
https://www3.ntu.edu.sg/home/ehchua/programming/index.html  
http://web.cs.ucla.edu/classes/fall14/cs143/project/cpp/gcc-intro.html  
https://courses.cs.washington.edu/courses/cse451/99wi/Section/gccintro.html 
https://www.geeksforgeeks.org/compiling-with-g-plus-plus/  
https://riptutorial.com/cplusplus/example/1334/compiling-with-gcc  
https://riptutorial.com/gcc  
https://web.stanford.edu/class/cs107/resources/gcc  
