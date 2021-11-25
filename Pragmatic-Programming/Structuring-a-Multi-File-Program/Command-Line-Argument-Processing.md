# Command Line Argument Processing

## argc, argv and envp
The ```main()``` function has two arguments that are traditionally called ```argc``` (argument count) and ```argv``` (argument vector), although the compiler does not require these names. For e.g. you can have ``int argi`` and ``char *args[]`` inplace of ``int argc`` and ``char *argv[]`` respectively. The types for command line arguments are defined by the language.  
 
 ``argc``, is a non-negative number corresponding to the number of elements in ``argv``.  
 
``argv`` is an array of pointers to null-terminated strings that correspond to the program arguments. Usually, an operating system passes the full path to the program’s
executable as the first argument i.e. ``argv[0]``. The last argument from the command line is ``argv[argc - 1]``, and ``argv[argc]`` is always NULL. For example if ``a.out`` be the program being executed and it is passed the following command line arguments:

```Console
$:~ a.out foo 28 M
```

Then ```argv = ["/home/malagi/a.out", "foo" "28"]``` and ```argc=3```. 
 
Additional arguments known as implementation parammeters can be passed to ``main``. Implementation parameters aren’t common in modern desktop environments. Traditionally, if a third parameter is passed to ``main``, that parameter is named ``envp`` and contains an array of null terminated strings representing the variables set in the user's environment. The environment block passed to ``main`` is a "frozen" copy of the current environment. Later changes to the environment won't change the values in ``envp``.
 
| Argument | Description                                                                             |
|----------|-----------------------------------------------------------------------------------------|
| argc     | Number of elements in the argument vector                                               |
| argv     | Array of character pointers corresponding to the program arguments                      |
| envp     | Array of character pointers corresponding to the environment variables and their values |

The following ignores any command line arguments:

```C++
// no arguments, ignore any command line arguments or environmental variables
void main() { /* body */ } 
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

## Example to print command line arguments
```C++
#include <iostream>

using std::cout;
using std::endl;

int main(int argc, char *argv[], char *envp[])
{
  // print command line arguments
  cout << "-----------------------------------------------------------------" << endl;
  cout << "Command line arguments: " << endl;
  cout << "-----------------------------------------------------------------" << endl;
  for (int i = 0; i < argc; ++i)
    cout << "argv["<< i << "]: " << argv[i] << endl;
  cout << endl;
  return 0;
}
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
  cout << "-----------------------------------------------------------------" << endl;
  cout << "Environment contains: " << endl;
  cout << "-----------------------------------------------------------------" << endl;
  while (*envp != NULL)
    cout << (*envp++);
  cout << endl;
  return 0;
}
```
