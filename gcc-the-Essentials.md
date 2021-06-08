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
