# Doxygen

Doxygen is the de facto standard tool for generating documentation from annotated C/C++ sources. Since the documentation is extracted directly from the source, it is much easier to keep the documentation consistent with the source code. Doxygen offers the following capabilities:

* generate an on-line documentation browser (in HTML) and/or an off-line reference manual (e.g. LaTeX, man pages) from a set of documented source files
* quickly extract code structure from undocumented source files which is often useful in navigating large code bases
* visualize relationships between between various elements by means of include dependency graphs, inheritance diagrams, collaboration diagrams etc. 

``doxygen`` - the executable ``doxygen`` is the main program that parses the sources and generates documentation  
``doxywizard`` - is a graphical front-end for editing the configuration file that is used by ``doxygen`` and for running ``doxygen`` in a graphical environment

Working with Doxygen in a new project basically involves three steps:

* Step I - annotate source code with special comments or comment blocks which Doxygen can understand
* Step II - create a configuration file
* Step III - run ``doxygen`` to parse the source code and generate documentation
 
#### Step 1: Creating a configuration file

https://www.doxygen.nl/manual/starting.html
Doxygen uses a configuration file to determine all of its settings. Each project should get its own configuration file. A project can consist of a single source file, but can also be an entire source tree that is recursively scanned.

To simplify the creation of a configuration file, doxygen can create a template configuration file for you. To do this call doxygen from the command line with the -g option:

doxygen -g <config-file>

where <config-file> is the name of the configuration file. If you omit the file name, a file named Doxyfile will be created. If a file with the name <config-file> already exists, doxygen will rename it to <config-file>.bak before generating the configuration template.
  
The configuration file has a format that is similar to that of a (simple) Makefile. It consists of a number of assignments (tags) of the form:

TAGNAME = VALUE or
TAGNAME = VALUE1 VALUE2 ...

You can probably leave the values of most tags in a generated template configuration file to their default value.  
  https://www.doxygen.nl/manual/config.html
  
If you do not wish to edit the configuration file with a text editor, you should have a look at doxywizard, which is a GUI front-end that can create, read and write doxygen configuration files, and allows setting configuration options by entering them via dialogs.
  
For a small project consisting of a few C and/or C++ source and header files, you can leave INPUT tag empty and doxygen will search for sources in the current directory.

If you have a larger project consisting of a source directory or tree you should assign the root directory or directories to the INPUT tag, and add one or more file patterns to the FILE_PATTERNS tag (for instance *.cpp *.h). Only files that match one of the patterns will be parsed (if the patterns are omitted a list of typical patterns is used for the types of files doxygen supports). For recursive parsing of a source tree you must set the RECURSIVE tag to YES. To further fine-tune the list of files that is parsed the EXCLUDE and EXCLUDE_PATTERNS tags can be used. To omit all test directories from a source tree for instance, one could use:

EXCLUDE_PATTERNS = */test/*


#### Step II

  Step 2: Running doxygen

To generate the documentation you can now enter:

doxygen <config-file>

Depending on your settings doxygen will create html, rtf, latex, xml, man, and/or docbook directories inside the output directory.he default output directory is the directory in which doxygen is started. The root directory to which the output is written can be changed using the OUTPUT_DIRECTORY. The format specific directory within the output directory can be selected using the HTML_OUTPUT, RTF_OUTPUT, LATEX_OUTPUT, XML_OUTPUT, MAN_OUTPUT, and DOCBOOK_OUTPUT. tags of the configuration file.
  
  HTML output

The generated HTML documentation can be viewed by pointing a HTML browser to the index.html file in the html directory.

#### Step 3
  
Step 3: Documenting the sources

Although documenting the sources is presented as step 3, in a new project this should of course be step 1. Here I assume you already have some code and you want doxygen to generate a nice document describing the API and maybe the internals and some related design documentation as well.

If the EXTRACT_ALL option is set to NO in the configuration file (the default), then doxygen will only generate documentation for documented entities. So how do you document these? For members, classes and namespaces there are basically two options:

    Place a special documentation block in front of the declaration or definition of the member, class or namespace. For file, class and namespace members it is also allowed to place the documentation directly after the member.

    See section Special comment blocks to learn more about special documentation blocks.

    Place a special documentation block somewhere else (another file or another location) and put a structural command in the documentation block. A structural command links a documentation block to a certain entity that can be documented (e.g. a member, class, namespace or file).

    See section Documentation at other places to learn more about structural commands.

The advantage of the first option is that you do not have to repeat the name of the entity.

Files can only be documented using the second option, since there is no way to put a documentation block before a file. Of course, file members (functions, variables, typedefs, defines) do not need an explicit structural command; just putting a special documentation block in front or behind them will work fine.
  
  https://www.doxygen.nl/manual/docblocks.html#structuralcommands
  
## Special Comment Blocks
https://www.doxygen.nl/manual/docblocks.html#specialblock  
https://www.doxygen.nl/manual/docblocks.html  
  
  
## Call graphs
  https://www.doxygen.nl/manual/diagrams.html
https://stackoverflow.com/questions/8887810/how-to-get-doxygen-to-produce-call-caller-graphs-for-c-functions  
  https://stackoverflow.com/questions/63484344/create-a-call-graph-for-a-specific-function-using-doxygen  
  https://dzone.com/articles/understanding-code-call-graphs
  https://www.programmersought.com/article/56204839816/  
  https://www.programmersought.com/article/20004640526/
