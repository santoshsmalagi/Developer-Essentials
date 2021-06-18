# GCC - the Essentials

This tutorial focusses on the following topics:
1. The GNU Compiler Collection (GCC)
2. GNU C and C++ compilers - gcc and g++
3. Compiling C programs using gcc
4. Compiling C++ programs using g++
5. Options controlling output generation
6. Options for diagnostics and warning messages
7. Static analyzer options
8. Debugging options
9. Optimization options
10. Code coverage and profiling options
11. Preprocessor options
12. Platform specific options
13. Static and Dynamic libraries
14. Summary
15. Common File Formats
16. Common compiler flags

> *Note: In all subsequent discussions, GCC - refers to the GNU Compiler Collection; gcc - refers to the GNU C Compiler and g++ is the GNU C++ Compiler. When we talk about compiling one of those languages, we might refer to that compiler by its own name, or as GCC. Either is correct!

## 1. The GNU Compiler Collection (GCC)

GCC stands for “GNU Compiler Collection”. GCC is an integrated distribution of compilers for several major programming languages. These languages currently include C, C++,
Objective-C, Objective-C++, Fortran, Ada, D, Go, and BRIG (HSAIL).  

The language-independent component of GCC includes the majority of the optimizers, as well as the *“back ends”* that generate machine code for various processors.  The part of a compiler that is specific to a particular language is called the *“front end”*. For example the C front-end is called CC, the C++ compiler is G++, the Ada compiler is GNAT, and so on.  

Historically, compilers for many languages, including C++ and Fortran, have been implemented as “preprocessors” which emit another high level language such as C. None of
the compilers included in GCC are implemented this way; they all generate machine code directly.

## 2. GNU C and C++ compilers - gcc and g++

When you compile C++ programs, you should invoke GCC as g++ instead.

C++ source files conventionally use one of the suffixes ‘.C’, ‘.cc’, ‘.cpp’, ‘.CPP’, ‘.c++’,
‘.cp’, or ‘.cxx’; C++ header files often use ‘.hh’, ‘.hpp’, ‘.H’, or (for shared template code)
‘.tcc’; and preprocessed C++ files use the suffix ‘.ii’. GCC recognizes files with these
names and compiles them as C++ programs even if you call the compiler the same way as
for compiling C programs (usually with the name gcc).

However, the use of gcc does not add the C++ library. g++ is a program that calls GCC
and automatically specifies linking against the C++ library. It treats ‘.c’, ‘.h’ and ‘.i’ files
as C++ source files instead of C source files unless ‘-x’ is used. This program is also useful
when precompiling a C header file with a ‘.h’ extension for use in C++ compilations. On
many systems, g++ is also installed with the name c++.

#### C Standards supported by GCC

GCC supports all C standards starting ANSI C Standard in 1990 upto more recent C2X standard, which has mostly experimental support. GCC also provides some extensions to the C language - GNU Extensions that, on rare occasions conflict with the C standard. Use of the ‘-std’ options listed below disables these extensions where they conflict with the C standard version selected.

|  C Standard |  Language option  | GNU Extension |
|:-----------:|:-----------------:|:-------------:|
| ANSI or C90 | -ansi or -std=c90 |  -std=gnu90   |
|     C99     |      -std=c99     |  -std=gnu99   |
|     C11     |      -std=c11     |  -std=gnu03   |
|     C17     |      -std=c17     |  -std=gnu17   |
|     C2X     |      -std=c2x     |  -std=gnu2x   |

> *The default, if no C language dialect options are given, is ‘-std=gnu17’.*

#### C++ Standards supported by GCC

GCC supports the original ISO C++ standard published in 1998, and the 2011, 2014, 2017 and mostly 2020 revisions. By default, GCC also provides some additional extensions to the C++ language that on rare occasions conflict with the C++ standard. Use of the ‘-std’ options listed below disables these extensions where they they conflict with the C++ standard version selected.

|        C++ Standard       |   Language option   | GNU Extension |
|:-------------------------:|:-------------------:|---------------|
| ISO C++ Standard or C++98 | -ansi or -std=c++98 | -std=gnu++98  |
|           C++03           |      -std=c++03     | -std=gnu++03  |
|           C++11           |      -std=c++11     | -std=gnu++11  |
|           C++14           |      -std=c++14     | -std=gnu++14  |
|           C++17           |      -std=c++17     | -std=gnu++17  |
|           C++20           |      -std=c++20     | -std=gnu++20  |

> *The default, if no C language dialect options are given, is ‘-std=gnu++17’.*

 ## 3. Compiling C Programs with gcc
 
After you edit your source code with a text-editor, the next step is to to compile your program using GCC.  When you invoke GCC, it normally does preprocessing, compilation, assembly and linking.


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

## 4. Compiling C++ Programs with g++

## 5. Options controlling output generation
Most of the command-line options that you can use with GCC are useful for C programs; when an option is only useful with another language (usually C++), the explanation says
so explicitly.

The gcc program accepts options and file names as operands.

You can mix options and other arguments. For the most part, the order you use doesn’t matter.

Order does matter when you use several options of the same kind.

Some options take one or more arguments typically separated either by a space or by the equals sign (‘=’) from the option name.

Compilation can involve up to four stages: preprocessing, compilation proper, assembly and linking, always in that order. The “overall options” allow you to stop this process at an intermediate stage. GCC is capable of preprocessing and compiling several files either into several assembler input files, or into one assembler input file; then each assembler input file produces an object file, and linking combines all the object files (those newly compiled, and those specified as input) into an executable file. 

-c Compile or assemble the source files, but do not link. The ultimate output is in the form of an object file for each source
file. By default, the object file name for a source file is made by replacing the suffix
‘.c’, ‘.i’, ‘.s’, etc., with ‘.o’. Unrecognized input files, not requiring compilation or assembly, are ignored.

-S Stop after the stage of compilation proper; do not assemble. The output is in
the form of an assembler code file for each non-assembler input file specified.
By default, the assembler file name for a source file is made by replacing the
suffix ‘.c’, ‘.i’, etc., with ‘.s’.
Input files that don’t require compilation are ignored.

-E Stop after the preprocessing stage; do not run the compiler proper. The output
is in the form of preprocessed source code, which is sent to the standard output.
Input files that don’t require preprocessing are ignored.
-o file Place the primary output in file file. This applies to whatever sort of output is
being produced, whether it be an executable file, an object file, an assembler
file or preprocessed C code.

If ‘-o’ is not specified, the default is to put an executable file in ‘a.out’, the
object file for ‘source.suffix’ in ‘source.o’, its assembler file in ‘source.s’, a
precompiled header file in ‘source.suffix.gch’, and all preprocessed C source
on standard output.

C/C++ language Options

-std= Determine the language standard

Directory options

These options specify directories to search for header files, for libraries and for parts of the
compiler:

-I dir
-iquote dir
-isystem dir
-idirafter dir
Add the directory dir to the list of directories to be searched for header files during
preprocessing. If dir begins with ‘=’ or $SYSROOT, then the ‘=’ or $SYSROOT
is replaced by the sysroot prefix; see ‘--sysroot’ and ‘-isysroot

## 6. Options for diagnostics and warning messages

Warnings are diagnostic messages that report constructions that are not inherently erroneous
but that are risky or suggest there may have been an error.

-w Inhibit all warning messages.
-Werror Make all warnings into errors.
-Werror= Make the specified warning into an error. The specifier for a warning is
appended; for example ‘-Werror=switch’ turns the warnings controlled by
‘-Wswitch’ into errors.

-Wall This enables all the warnings about constructions that some users consider
questionable, and that are easy to avoid (or modify to prevent the warning),
even in conjunction with macros. This also enables some language-specific
warnings

-Wextra This enables some extra warning flags that are not enabled by ‘-Wall’.
The option ‘-Wextra’ also prints warning messages for the following cases:
 A pointer is compared against integer zero with <, <=, >, or >=.
 (C++ only) An enumerator and a non-enumerator both appear in a conditional
expression.
 (C++ only) Ambiguous virtual bases.
 (C++ only) Subscripting an array that has been declared register.
 (C++ only) Taking the address of a variable that has been declared
register.
 (C++ only) A base class is not initialized in the copy constructor of a derived
class.

## 7. Static analyzer options

## 8. Debugging options

To tell GCC to emit extra information for use by a debugger, in almost all cases you need
only to add ‘-g’ to your other options.
GCC allows you to use ‘-g’ with ‘-O’. The shortcuts taken by optimized code may
occasionally be surprising: some variables you declared may not exist at all; flow of control
may briefly move where you did not expect it; some statements may not be executed because
they compute constant results or their values are already at hand; some statements may
execute in different places because they have been moved out of loops. Nevertheless it
is possible to debug optimized output. This makes it reasonable to use the optimizer for
programs that might have bugs.
If you are not using some other optimization option, consider using ‘-Og’

With no ‘-O’ option at all, some compiler passes
that collect information useful for debugging do not run at all, so that ‘-Og’ may result in
a better debugging experience.
-g Produce debugging information in the operating system’s native format (stabs,
COFF, XCOFF, or DWARF). GDB can work with this debugging information.
On most systems that use stabs format, ‘-g’ enables use of extra debugging
information that only GDB can use; this extra information makes debugging
work better in GDB but probably makes other debuggers crash or refuse to read
the program.

-ggdb Produce debugging information for use by GDB. This means to use the most
expressive format available (DWARF, stabs, or the native format if neither of
those are supported), including GDB extensions if at all possible.
gdwarf
-gdwarf-version
Produce debugging information in DWARF format (

-glevel
-ggdblevel

Request debugging information and also use level to specify how much information.
The default level is 2.
Level 0 produces no debug information at all. Thus, ‘-g0’ negates ‘-g’.
Level 1 produces minimal information, enough for making backtraces in parts
of the program that you don’t plan to debug. This includes descriptions of
functions and external variables, and line number tables, but no information
about local variables.
Level 3 includes extra information, such as all the macro definitions present in
the program. Some debuggers support macro expansion when you use ‘-g3’.
If you use multiple ‘-g’ options, with or without level numbers, the last such
option is the one that is effective.

## 9. Optimization options

These options control various sorts of optimizations.
Without any optimization option, the compiler’s goal is to reduce the cost of compilation
and to make debugging produce the expected results

Turning on optimization flags makes the compiler attempt to improve the performance
and/or code size at the expense of compilation time and possibly the ability to debug the
program.

Most optimizations are completely disabled at ‘-O0’ or if an ‘-O’ level is not set on the
command line, even if individual optimization flags are specified. Similarly, ‘-Og’ suppresses
many optimization passes.

-O
-O1 Optimize. Optimizing compilation takes somewhat more time, and a lot more
memory for a large function.

-O2 Optimize even more. GCC performs nearly all supported optimizations that do
not involve a space-speed tradeoff. As compared to ‘-O’, this option increases
both compilation time and the performance of the generated code

-O3 Optimize yet more. ‘-O3’ turns on all optimizations specified by ‘-O2’ and also
turns on the following optimization flags:

-O0 Reduce compilation time and make debugging produce the expected results.
This is the default.

-Og Optimize debugging experience. ‘-Og’ should be the optimization level of choice
for the standard edit-compile-debug cycle, offering a reasonable level of optimization
while maintaining fast compilation and a good debugging experience

> For a complete list of optimization flags turned on by gcc refer the gcc manual

## 10. Code coverage and profiling options

purpose of instrumentation is collect profiling statistics for use in finding program hot spots, code coverage analysis, or profile-guided optimizations.

-p
-pg Generate extra code to write profile information suitable for the analysis program
prof (for ‘-p’) or gprof (for ‘-pg’). You must use this option when
compiling the source files you want data about, and you must also use it when
linking

## 11. Preprocessor options

These options control the C preprocessor, which is run on each C source file before actual
compilation. If you use the ‘-E’ option, nothing is done except preprocessing. Some of these options
make sense only together with ‘-E’ because they cause the preprocessor output to be unsuitable
for actual compilation.

-D name Predefine name as a macro, with definition 1.
-D name=definition
The contents of definition are tokenized and processed as if they appeared during
translation phase three in a ‘#define’ directive.

## 12. Platform specific options
## 13. Static and Dynamic libraries
## 14. Summary
## 15. Common File Formats

file.c C source code that must be preprocessed.
file.i C source code that should not be preprocessed.
file.ii C++ source code that should not be preprocessed

file.h C, C++, Objective-C or Objective-C++ header file to be turned into a precompiled
header (default), or C, C++ header file to be turned into an Ada spec (via
the ‘-fdump-ada-spec’ switch).
file.cc
file.cp
file.cxx
file.cpp
file.CPP
file.c++
file.C C++ source code that must be preprocessed. Note that in ‘.cxx’, the last two
letters must both be literally ‘x’. Likewise, ‘.C’ refers to a literal capital C.
file.hh
file.H
file.hp
file.hxx
file.hpp
file.HPP
file.h++
file.tcc C++ header file to be turned into a precompiled header

file.s Assembler code.
file.S
file.sx Assembler code that must be preprocessed.

## 16. Common compiler flags
C

    Program for compiling C programs; default ‘cc’.
CXX

    Program for compiling C++ programs; default ‘g++’.
CPP

    Program for running the C preprocessor, with results to standard output; default ‘$(CC) -E’
    CFLAGS

    Extra flags to give to the C compiler.
CXXFLAGS

    Extra flags to give to the C++ compiler.
CPPFLAGS

    Extra flags to give to the C preprocessor and programs that use it (the C and Fortran compilers).
LDFLAGS

    Extra flags to give to compilers when they are supposed to invoke the linker, ‘ld’, such as -L. Libraries (-lfoo) should be added to the LDLIBS variable instead.
LDLIBS

    Library flags or names given to compilers when they are supposed to invoke the linker, ‘ld’. LOADLIBES is a deprecated (but still supported) alternative to LDLIBS. Non-library linker flags, such as -L, should go in the LDFLAGS variable.
LFLAGS

    Extra flags to give to Lex.

The CFLAGS variable sets compile flags for gcc:
The LDFLAGS variable sets flags for linker

https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html  
https://www3.ntu.edu.sg/home/ehchua/programming/index.html  
http://web.cs.ucla.edu/classes/fall14/cs143/project/cpp/gcc-intro.html  
https://courses.cs.washington.edu/courses/cse451/99wi/Section/gccintro.html   
https://www.geeksforgeeks.org/compiling-with-g-plus-plus/  
https://riptutorial.com/cplusplus/example/1334/compiling-with-gcc  
https://riptutorial.com/gcc  
https://web.stanford.edu/class/cs107/resources/gcc  
