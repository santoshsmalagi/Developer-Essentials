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

