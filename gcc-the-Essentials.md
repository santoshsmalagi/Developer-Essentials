# GCC - the Essentials

After you edit your source code with a text-editor, the next step is to to compile your program using one of the following commands.

```Shell
g++ -std=<std> -o <executable> <src1.cpp> [src2.cpp ...]              # to compile witha specific version of C++
g++ -Wall -g -o <executable> <src1.cpp> [src2.cpp ...]                # compile with warnings and -g flags to enable debug build (i.e. include debugging symbols)
```

Run the program as follows:

```Shell
./<executable>
```
