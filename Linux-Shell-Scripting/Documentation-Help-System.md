# Linux Documentation and Help System

#### Getting help for shell builtins (help)
```bash``` has a built-in help facility for each of the shell built-in commands, which can be invoked by using ```help``` followed by the name of the shell built-in.

```console
help <shell builtin command>
```

#### *--help* - display usage information
Many executable programs support a --help option that displays a description of the command’s supported syntax and options. For example:
```console
mkdir --help
```
*All programs don't support the ```--help``` option.*

#### Displaying a program's man page
Most executable programs intended for command-line use provide a formal piece of documentation called a manual or man page. A special paging program
called ```man``` is used to view them, like this:

```console
man program
```
where program is the name of the command to view. Man pages vary somewhat in format but generally contain a title, a synopsis of the command’s syntax, a description of the command’s purpose, and a listing and description of each of the command’s options. Man pages, however, do not usually include examples, and they are intended as a reference,
not a tutorial.

**Sections in the "manual"**  
The "manual" that ```man``` displays is broken into sections and covers not only user commands but also system administration commands, programming interfaces, file formats, and more.

| Section | Contents                                       |
|---------|------------------------------------------------|
| 1       | User commands                                  |
| 2       | Programming interfaces for kernel system calls |
| 3       | Programming interfaces to the C library        |
| 4       | Special files such as device nodes and drivers |
| 5       | File formats                                   |
| 6       | Games and amusements such as screensavers      |
| 7       | Miscellaneous                                  |
| 8       | System administration commands                 |

Sometimes a command as well as a file format can have the same names, in such situations we need to browse through specific sections of the manual.  no section is specified then the first instance of a match is displayed.

```console
man 1 passwd             # displays man page for the passwd command i.e. passwd - change user passwd
man 5 passwd             # displays the format of /etc/passwd file i.e. passwd - the paswd file
```

> **INFO** pages: GNU provides an alternative to man pages called *info pages*. Info pages are hyperlinked much like web pages. The ```info``` program reads info files, which are tree-structured into individual nodes, each containing a single topic. Try, ```info coreutils```.

**README and Other Documentation**  
Many software packages installed on the system have documentation files residing in the ```/usr/share/doc``` directory.

### Displaying appropriate commands (apropos)
It is also possible to search the list of man pages for possible matches based on a search term.

```console
apropos <search term>
man -k <search term>
```

For example:
```console
apropos delete           # displays all relevant matches in the man file corresponding to the search term delete
```

### Display a brief command description (whatis)
The whatis program displays the name and a one-line description of a man page matching a specified keyword
```console
santoshsmalagi@remotedesktop:~$ whatis passwd
passwd (5)    - the passwd file
passwd (1)    - change user password
passwd (1ssl) - compute password hashes
```
### Displaying a command's location (which)
The ```which``` command can determine the location of an executable, this is helpful when more than one version of an executable program is installed on a system. The ```which``` works only for executable programs, not builtins or aliases.

```console
which <command>
```
