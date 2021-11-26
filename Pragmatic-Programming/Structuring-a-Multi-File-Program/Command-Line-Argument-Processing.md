# Command Line Argument Processing

## argc, argv, and envp
The ```main()``` function has two arguments that are traditionally called ```argc``` (argument count) and ```argv``` (argument vector), although the compiler does not require these names. For e.g. you can have ``int argi`` and ``char *args[]`` inplace of ``int argc`` and ``char *argv[]`` respectively. The types for command line arguments are defined by the language.  

> *Note that ``main`` can accept any number of command line arguments.*
 
 ``argc``, is a non-negative number corresponding to the number of elements in ``argv`` and is calculated automatically.
 
``argv`` is an array of pointers to null-terminated strings that correspond to the program arguments. 

Additional arguments known as implementation parammeters can be passed to ``main``. Implementation parameters aren’t common in modern desktop environments. Traditionally, if a third parameter is passed to ``main``, that parameter is named ``envp`` and contains an array of null terminated strings representing the variables set in the user's environment. The environment block passed to ``main`` is a "frozen" copy of the current environment. Later changes to the environment won't change the values in ``envp``.
 
| Argument | Description                                                                             |
|----------|-----------------------------------------------------------------------------------------|
| argc     | Number of elements in the argument vector                                               |
| argv     | Array of character pointers corresponding to the program arguments                      |
| envp     | Array of character pointers corresponding to the environment variables and their values |

The following ignores any command line arguments:

```C++
// no return value - C compiler will give a WARNING, NOT ALLOWED in C++
void main() { /* body */ } 
 
// no arguments, ignore any command line arguments or environmental variables
int main () { /* body */ }
```
The compiler expects a ```main()``` function in one of the following forms to be able to parse and process command line arguments:

```C++
// process command line arguments
int main (int argc, char *argv[]) { /* body */ }
 
// alternative to char *argv[], pointer form
int main (int argc, char **argv) { /* body */ }
 
// some implementations may require argv to be of const type
int main (int argc, const char **argv) { /* body */ }                   

// envp provides a list of environment variables and their values
int main (int argc, char *argv[], char *envp[]) { /* body */ }         
```
## Argument processing
* Arguments are delimited by white space, which is either a space or a tab.
* Usually, the operating system passes the full path to the program’s executable as the first argument i.e. ``argv[0]``.
* The last argument from the command line is ``argv[argc - 1]``, and ``argv[argc]`` is always NULL.
* Quotes
    * Quotes around the name of the program executable (e.g. ``"./a.out"``) are allowed. 
    * However, the double quote marks aren't included in the ``argv[0]`` i.e. ``argv[0] = a.out`` in this case.
    * A string surrounded by single or double quotes is interpreted as a single argument, which may contain white-space characters (see examples below)
 * Backslashes
     * interpreted literally, unless they immediately precede a double quote mark.
     * one backslash (\) is placed in the ``argv`` array for every pair of backslashes (\\)
     * for an odd number of backslashes, one backslash is placed for every pair and additional backslash is ignored, e.g. ``a\\\b`` becomes ``a\b`` 

## Example to print command line arguments
```C++
#include <iostream>

using std::cout;
using std::endl;

int main(int argc, char *argv[], char *envp[])
{
  // print command line arguments
  cout << "-----------------------------------------------" << endl;
  cout << "Command line arguments: " << endl;
  cout << "-----------------------------------------------" << endl;
  for (int i = 0; i < argc; ++i)
    cout << "argv["<< i << "]: " << argv[i] << endl;
  cout << endl;
  return 0;
}
```

Example output,

```Shell

./parse
-----------------------------------------------------------------
Command line arguments: 
-----------------------------------------------------------------
argv[0]: ./parse

"./parse" abc de"f"
-----------------------------------------------------------------
Command line arguments: 
-----------------------------------------------------------------
argv[0]: ./parse
argv[1]: abc
argv[2]: def

./parse ab a\b a\\b a\\\b a\\\\b a\\\\\b
-----------------------------------------------------------------
Command line arguments: 
-----------------------------------------------------------------
argv[0]: ./parse
argv[1]: ab
argv[2]: ab
argv[3]: a\b
argv[4]: a\b
argv[5]: a\\b
argv[6]: a\\b

./parse a b cd"e f g" hi 'j k'
-----------------------------------------------------------------
Command line arguments: 
-----------------------------------------------------------------
argv[0]: ./parse
argv[1]: a
argv[2]: b
argv[3]: cde f g
argv[4]: hi
argv[5]: j k
```

## Example to print environment variables

```C++
#include <iostream>
#include <cstring>

using std::cout;
using std::endl;

int main(int argc, char *argv[], char *envp[])
{
  bool printLineNumbers {false};
  int i {};

  if ((argc == 2) && (strcmp(argv[1], "/n") == 0))
    printLineNumbers = true;

  while (envp[i] != NULL)
  {
    if (printLineNumbers)
      cout << i << ".";
    cout << envp[i] << endl;
    ++i;
  }

  return 0;
}
```

## Another example to print environment variables using postfix expression

```C++
#include <iostream>

using std::cout;
using std::endl;

int main(int argc, char *argv[], char *envp[])
{
  // print environment variables
  cout << "---------------------------------------" << endl;
  cout << "Environment contains: " << endl;
  cout << "---------------------------------------" << endl;
  while (*envp != NULL)
    cout << (*envp++);
  cout << endl;
  return 0;
}
```

## Program to print a histogram of the length of command line arguments
```C++

// Use a map to store the lengths of the command line arguments, and
// number of times a string with particular length appears as a <key,value> pair
// were the key is the string lengths and value is the number of time it appears

#include <iostream>
#include <map>
#include <utility>
#include <cstring>

using std::cout;
using std::endl;

int main(int argc, char *argv[])
{
  std::map<int,int> argLengthsMap;
  std::map<int,int>::iterator argLengthsMapIter;
  
  if (argc == 1)
  {
    cout << "No arguments to process..!";
    return 0;
  }
  
  for (int i = 1; i < argc; ++i)
  {
    argLengthsMapIter = argLengthsMap.find(strlen(argv[i]));
    if (argLengthsMapIter == argLengthsMap.end())
      argLengthsMap.insert(std::pair<int,int> (strlen(argv[i]), 1));
    else
      argLengthsMapIter->second++;
  }

  cout << "------------------------------------" << endl
       << "Argument Length Histogram" << endl
       << "------------------------------------" << endl;

  argLengthsMapIter = argLengthsMap.begin();
  while (argLengthsMapIter != argLengthsMap.end())
  {
    cout << argLengthsMapIter->first << ". ";
    for (int j = 0; j < argLengthsMapIter->second; ++j)
      cout << "*";
    cout << endl;
    argLengthsMapIter++;
  }
  
  return 0;
}
```

Example output,

```Shell
$ ./parse a b c ab abc ab ab ab ab ab ab abc abcd efrfgkwnfei a b c w
------------------------------------
Argument Length Histogram
------------------------------------
1. *******
2. *******
3. **
4. *
11. *
```
