# GCC - the Essentials

After you edit your source code with a text-editor, the next step is to to compile your program using one of the following commands.

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
