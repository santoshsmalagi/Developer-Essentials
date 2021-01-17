 # Structuring a Multi-File C Program

The number of functions in one file must be kept at a minimum. This can be achieved by grouping related (or similar) functions together in related files. Start by laying out a directory structure with a number of empty files to start with, at a bare minimum:

* main.c
* main.h
* basic.c
* basic.h
* advanced.c
* advanced.h
* Makefile
