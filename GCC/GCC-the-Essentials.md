# GCC - the Essentials

> *GCC - refers to the GNU Compiler Collection; gcc - refers to the GNU C Compiler (or rightfully the C front-end to GCC) and g++ is the GNU C++ Compiler (or rightfully the C++ front-end to GCC). *cc* and *c++* are synonyms for gcc and g++ respectively. When we talk about compiling one of those languages, we might refer to that compiler by its own name, or simply as GCC. Either is correct!*

This tutorial focusses on the following topics:

* [1. The GNU Compiler Collection](/GCC/01.The-GNU-Compiler-Collection.md)
     * GNU Toolchain
     * History of GCC 
     * Major features of GCC
     * GCC Command Line Options
     * Specifying Command Line Options
     * GCC Platform Support
* [2. Compiling C Programs](/GCC/02.Compiling-C-Programs.md)
    * Compiling a simple C Program
    * Compiling multiple source files
    * Creating object files from source files
    * Creating executables by linking object files
    * Link order of object files
    * Specifying a C standard using ``-std`` command option
* [3. Compiling C++ Programs](/GCC/03.Compiling-C++Programs.md)
    * Compiling a simple C++ Program
    * Compiling multiple source files
    * Creating object files from source files
    * Creating executables by linking object files
    * Link order of object files
    * gcc vs g++
    * Specifying a C++ standard using ``-std`` command option
* [4. Options controlling output generation](/GCC/04.Options-controlling-output-generation.md)
    * Compile or assemble the source files, but do not link (-c)
    * Stop after the stage of compilation proper, do not assemble (-S)
    * Stop after the preprocessing stage, do not run the compiler proper ``(-E)`` 
    * Place the primary output in the specified output file ``(-o)``
    * Directory search path ``(-I)``
* [5. Options for warning messages](/GCC/05.Options-for-warning-messages.md)
* [6. Static analyzer options](/GCC/06.Static-analyzer-options.md)
* [7. Debugging options](/GCC/07.Debugging-options.md)
    * Why debugging optimized code is not always efficient?
    * Commonly used debug options
* [8. Code coverage and profiling options](08.Code-coverage-and-profiling-options.md)
* [9. Optimization options](/GCC/09.Optimization-options.md)
    * GCC Optimization levels
    * Optimization techniques
* [10. Preprocessor options](/GCC/10.Preprocessor-options.md)
    * Defining macros during compilation
* [11. Platform specific options](/GCC/11.Platform-specific-options.md)
* [12. Examining object files and binaries ](/GCC/12.Examining-object-files-and-binaries.md)
    * ``file``
    * ``size``
    * ``nm``
    * ``strip``
    * ``strace``
    * ``ltrace``
    * ``ldd``
    * ``objdump``
    * ``readelf`` 
* [13. Common File Formats](/GCC/13.Common-File-Formats.md)  
* [14. Common compiler flags](/GCC/14.Common-compiler-flags.md)  


https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html  
https://riptutorial.com/cplusplus/example/1334/compiling-with-gcc  
https://riptutorial.com/gcc  
https://web.stanford.edu/class/cs107/resources/gcc  
