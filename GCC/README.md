# The Minimalist GCC Setup

Configure the following environment variables to point to your GCC install path for a minimalist GNU C/C++ toolchain setup:

```bash
#!/bin/bash

GCC_INSTALL_DIR=<path to GCC install dir>

# setup PATH to GCC install dir
export PATH=${GCC_INSTALL_DIR}/bin:${PATH}

# list of dir path to search for header files
# to be more specific you can use C_INCLUDE_PATH (for C headers) and CPLUS_INCLUDE_PATH (for C++ headers)
export CPATH=${GCC_INSTALL_DIR}/include/c++

# colon seperated list of dir paths to search for compile time static/shared libs
export LIBRARY_PATH=${GCC_INSTALL_DIR}/lib64

# colon seperated list of dir paths to search for runtime shared libs
export LD_LIBRARY_PATH=${GCC_INSTALL_DIR}/lib64
```

For more information on GCC environment variables, see:  
[Environment Variables Affecting GCC](https://gcc.gnu.org/onlinedocs/gcc/Environment-Variables.html)

## Example Search Paths for Headers and Libraries in Ubuntu

Run with ``-v`` flag turned on, so that GCC can report the search paths for headers and library object files:

```shell
$ g++ -Wall -v main.cpp -o main
```

#### Search paths for include headers

```shell
#include "..." search starts here:
#include <...> search starts here:
 /usr/include/c++/11
 /usr/include/x86_64-linux-gnu/c++/11
 /usr/include/c++/11/backward
 /usr/lib/gcc/x86_64-linux-gnu/11/include
 /usr/local/include
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
```

#### Search paths for libraries

```shell
LIBRARY_PATH=/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../lib/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../:/lib/:/usr/lib/
```
