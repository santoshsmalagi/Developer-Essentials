# Doxygen

Doxygen is the de facto standard tool for generating documentation from annotated C/C++ sources. Since the documentation is extracted directly from the source, it is much easier to keep the documentation in sync with the source code. Doxygen offers the following capabilities:

* generate an on-line documentation browser (in HTML) and/or an off-line reference manual (e.g. LaTeX, man pages) from a set of documented source files
* quickly extract code structure from undocumented source files, which is useful for navigating large code bases
* visualize relationships between between various elements by means of include dependency graphs, inheritance diagrams, collaboration diagrams etc. 

``doxygen`` - the executable ``doxygen`` is the main program that parses the sources and generates documentation  
``doxywizard`` - is a graphical front-end for editing the configuration file which is used by ``doxygen`` and for running ``doxygen`` in a graphical environment

Working with Doxygen basically involves three steps:

* Step I - annotate source code with *special comments* or *special comment blocks*
* Step II - create a configuration file
* Step III - run ``doxygen`` to parse the source code and generate documentation

## Annotating Source Code with Comment Blocks

Various entities in source code (e.g. classes and their member variables and methods, global functions, structs, variables, namespaces, macros and typedefs) need to be annotated with comments (or comment blocks). This can be done in two ways:

* Place a special comment block in front of the declaration or definition of the member, class or namespace. For file, class and namespace members it is also allowed to place the documentation directly after the member. See - [Special comment blocks](https://www.doxygen.nl/manual/docblocks.html#specialblock).
* Place a special comment block somewhere else (another file or another location) and put a structural command in the documentation block. A structural command links a documentation block to a certain entity that can be documented (e.g. a member, class, namespace or file). See - [Documentation at other places](https://www.doxygen.nl/manual/docblocks.html#structuralcommands) and [Special Commands](https://www.doxygen.nl/manual/commands.html).

A special comment block is a C or C++ style comment block with some additional markings, so doxygen knows it is a piece of structured text that needs to end up in the generated documentation. 

For each entity in the code there are two (or in some cases three) types of descriptions, which together form the documentation for that entity; a brief description and detailed description, both are optional. For methods and functions there is also a third type of description, the so called in body description, which consists of the concatenation of all comment blocks found within the body of the method or function.  

Having more than one brief or detailed description is allowed but not recommended.

## Configuration File

Doxygen uses a configuration file to determine all of its settings. Each project should get its own configuration file. A project can consist of a single source file, but can also be an entire source tree that is recursively scanned.

To simplify the creation of a configuration file, doxygen can create a template configuration file for you. To do this call doxygen from the command line with the -g option:

doxygen -g <config-file>

where <config-file> is the name of the configuration file. If you omit the file name, a file named Doxyfile will be created. If a file with the name <config-file> already exists, doxygen will rename it to <config-file>.bak before generating the configuration template.
  
The configuration file has a format that is similar to that of a (simple) Makefile. It consists of a number of assignments (tags) of the form:

TAGNAME = VALUE or
TAGNAME = VALUE1 VALUE2 ...

You can probably leave the values of most tags in a generated template configuration file to their default value. https://www.doxygen.nl/manual/config.html
  
If you do not wish to edit the configuration file with a text editor, you should have a look at doxywizard, which is a GUI front-end that can create, read and write doxygen configuration files, and allows setting configuration options by entering them via dialogs.
  
For a small project consisting of a few C and/or C++ source and header files, you can leave INPUT tag empty and doxygen will search for sources in the current directory.

If you have a larger project consisting of a source directory or tree you should assign the root directory or directories to the INPUT tag, and add one or more file patterns to the FILE_PATTERNS tag (for instance *.cpp *.h). Only files that match one of the patterns will be parsed (if the patterns are omitted a list of typical patterns is used for the types of files doxygen supports). For recursive parsing of a source tree you must set the RECURSIVE tag to YES. To further fine-tune the list of files that is parsed the EXCLUDE and EXCLUDE_PATTERNS tags can be used. To omit all test directories from a source tree for instance, one could use:

EXCLUDE_PATTERNS = */test/*

## Running Doxygen and Viewing Generated Output

```Shell
doxygen <config-file>
```

Depending on your settings doxygen will create html, rtf, latex, xml, man, and/or docbook directories inside the output directory.he default output directory is the directory in which doxygen is started. The root directory to which the output is written can be changed using the OUTPUT_DIRECTORY. The format specific directory within the output directory can be selected using the HTML_OUTPUT, RTF_OUTPUT, LATEX_OUTPUT, XML_OUTPUT, MAN_OUTPUT, and DOCBOOK_OUTPUT. tags of the configuration file.
 
#### HTML output
The generated HTML documentation can be viewed by pointing a HTML browser to the index.html file in the html directory.

  
## Generating Call Graphs
  https://www.doxygen.nl/manual/diagrams.html
  https://stackoverflow.com/questions/8887810/how-to-get-doxygen-to-produce-call-caller-graphs-for-c-functions  
  https://stackoverflow.com/questions/63484344/create-a-call-graph-for-a-specific-function-using-doxygen  
  https://dzone.com/articles/understanding-code-call-graphs
  https://www.programmersought.com/article/56204839816/  
  https://www.programmersought.com/article/20004640526/

      
## Installing and Building
## Advanced Topics
