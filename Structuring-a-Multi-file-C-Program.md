 # Structuring a Multi-File C Program

The number of functions in one file must be kept at a minimum. This can be achieved by grouping related (or similar) functions together in related files. Start by laying out a directory structure with a number of empty files to start with, at a bare minimum:

* main.c
* main.h
* basic.c
* basic.h
* advanced.c
* advanced.h
* Makefile
* README (optional)


If the programmer uses double-quotes around the name of the header file, the compiler will look for that file in the current directory. If the file is enclosed in <>, it will look for the file in a set of predefined directories.

The #ifdef, #define, #endif construction is collectively known as a "guard." This keeps the C compiler from including this file more than once per file. The compiler will complain if it finds multiple definitions/prototypes/declarations, so the guard is a must-have for header files.

* mmencode.h - MeowMeow, a stream encoder/decoder */
   
 #ifndef _MMENCODE_H
 #define _MMENCODE_H
   
 #include <stdio.h>
   
 int mm_encode(FILE *src, FILE *dst);
   
 #endif /* _MMENCODE_H */
