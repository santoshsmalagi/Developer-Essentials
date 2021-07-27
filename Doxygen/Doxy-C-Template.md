# Doxygen Template for C Programs
This is a collection of readily usable templates to generate documentation for C source code using the Doxygen system.  

Code documentation is absolutely vital to keep the codebase readable and to preserve valuable knowledge. While comments and code documentation are very important, the best code is self-documenting. When documenting code do it with the audience in mind - the next developer who will need to understand the code to fix bugs and ship enhancements. *Be generous — the next one may be you!*  

The following templates are mostly inspired from Steve Oualine's classic - *C Elements of Style* and *The Elements of C Programming Style* by Jay Ranade and Alan Nash.

## 1. Comments which should not be processed by Doxygen

For comments which need not be processed by Doxygen e.g. copyright notice use the boxed comment style:

```C
/***********************************************************************
* Copyright (C) 1988-2021 Free Software Foundation, Inc.               *
* This file is part of GCC.                                            *
* GCC is free software; you can redistribute it and/or modify it under *
* the terms of the GNU General Public License as published by the Free *
* Software Foundation; either version 3, or (at your option) any later *
* version.                                                             *
************************************************************************/
```

**IMPORTANT** - in your Doxyfile configurations ALWAYS set ``JAVADOC_BANNER = NO``, such that Doxygen will always interpret such boxed comments as *comments* and ignore them.

