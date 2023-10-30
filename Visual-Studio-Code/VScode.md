# Visual Studio Code
Visual Studio Code is a lightweight, modern, but powerful source code editor. It comes with built-in support for JavaScript, TypeScript and Node.js. Support for other languages and runtimes such as C++, C#, Java, Python, PHP, Go, .NET etc. is achieved by installing approriate extensions.

> *⚠️ The following tutorial focuses on setting up VS Code C/C++ development environment on Windows using the MinGW (GCC) toolchain. VS Code website provides examples for setting up C/C++ development environments for other platforms/compiler toolchains.*
 
## Why use Visual Studio Code for C/C++ Development?
* VS Code combines the simplicity of a source code editor with powerful developer tooling 
* It offers a seamless experience across Windows, Linux, and macOS.
* Features
  * syntax highlighting
  * bracket-matching
  * auto-indentation
  * box and snippets selection
  * IntelliSense code completion
  * rich semantic code understanding
  * support for code refactoring and navigation
* Interactive debugger
  * enabling developers to step through source code, inspect variables, view call stacks, and execute commands in the console
* VS Code also integrates with build and scripting tools

## VS Code Extensions
VS Code Extensions allow developers to extend the capability of VS Code by adding semantic language support, debuggers, and tools to support several development workflows. VS Code's rich extensibility model lets extension authors plug directly into the VS Code UI and contribute functionality through the same APIs used by VS Code. VS Code extensions can be installed directly using the [Visual Studio Code Marketplace](https://marketplace.visualstudio.com/VSCode). 

Some interesting VS Code Extensions for C/C++ development:
* [C/C++ for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
* [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
* [Peacock](https://marketplace.visualstudio.com/items?itemName=johnpapa.vscode-peacock)
* [vscodevim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)

## Visual Studio Code vs Vim?
Apart from the general look and feel, Vim and VS Code are two superior tools for C/C++ development in their own sense. To start with, Vim and VS Code use different keyboard shortcuts/key bindings. It is possible to emulate Vim *(or Emacs)* keyboard shortcuts in the VS Code environment. For example, using the [vscodevim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) extension, key bindings can be emulated to replicate Vim (though not all Vim features may be supported).

#### Where Vim stands out?
* **Command-line editing in Linux environment:** Vim *(or Emacs)* is the de-facto standard for text editing on the Linux terminal, especially if a GUI environment cannot be loaded.
* **Terminal operation inside the text editor:** Opening a terminal inside of Vim and running the debugger seemed more intutive in Vim vs VS Code. Even better, running gdb natively on Linux offered superior performance and control over running gdb inside of VS Code *(personal opinion)*.
* **System Programmers are used to Vim/Emacs:** Most Linux/System programmers are used to Vim/Emacs style code editing, as the two tools are natively available on every major Linux system.

#### Where VS Code stands out?
* **Outline View:** A notable feature of VS Code is it's ability to list an 'Outline' view of the current C/C++ source or header file and list symbols (functions, enums, structs, variables etc.) with icons in a nice outline view. Achieving similar functionality in Vim might require additional plugins/extra work.
* **Code Navigation:** Code navigation in Vim using [Exuberant ctags](https://ctags.sourceforge.net/) requires regeneration of a tag file everytime new code is checked out. VS Code automatically updates the references for declarations/definitions if new code is added to the developer work area. Besides code navigation/search features in VS Code stood out from Vim.
* **Version Control Extensions:** VS Code offers some powerful extensions for version control systems such as [Perforce for VS Code](https://marketplace.visualstudio.com/items?itemName=mjcrouch.perforce)

## Configuring Visual Studio Code for C/C++ Development

#### Step I - [Download and Install VS Code](https://code.visualstudio.com/docs/setup/setup-overview)
Provides step by step instruction for Windows, Linux, and macOS to download and install VS Code.

#### Step II - [C/C++ Extension for VS Code](https://code.visualstudio.com/docs/languages/cpp)
The next step is to enable C/C++ development support for VS Code by installing the [Microsoft C/C++ Extension](https://code.visualstudio.com/docs/languages/cpp) in VS Code. This extension is a *must have* for C/C++ development in VS Code and adds useful features such as syntax highlighting (colorization), smart completions and hovers (IntelliSense), error checking etc.

> *⚠️ The C/C++ extension doesn't include a C++ compiler or debugger.*

#### Step III - [Setup C/C++ Development Tools](https://code.visualstudio.com/docs/languages/cpp)
VS Code as an editor relies on command-line tools for C/C++ development workflow. The compiler-linker-debugger toolchain must therefore be installed, or VS Code must be configured to use these tools if they are already installed.

Open a VS Code terminal window by Ctrl+Shift+`. USe the following command to check if GCC has been installed on the system:
```bash
g++ --version
```

#### [Installing the MinGW-w64 toolchain](https://code.visualstudio.com/docs/cpp/config-mingw#_prerequisites)
The latest version of MinGW-w64 can be installed on Windows via MSYS2, which provides up-to-date native builds of GCC, MinGW-w64, and other helpful C++ tools and libraries to compile code, debug it, and configure it to work with IntelliSense.

> *[MSYS2](https://www.msys2.org/) is a collection of tools and libraries providing you with an easy-to-use environment for building, installing and running native Windows software. MSYS2 provides up-to-date native builds for GCC, mingw-w64, CPython, CMake, Meson, OpenSSL, FFmpeg, Rust, Ruby, just to name a few.*

#### Step IV (optional) - [Remote Development Setup](https://code.visualstudio.com/docs/remote/remote-overview)
The [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension pack allows you to open any folder in a container, on a remote machine, or in the Windows Subsystem for Linux (WSL) and take advantage of VS Code's full feature set. No source code needs to be on your local machine to gain these benefits since Remote Development runs commands and extensions directly on the remote machine.  

This Remote Development extension pack includes four extensions:
* [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) - Work with source code in any location by opening folders on a remote machine/VM using SSH. Supports x86_64, ARMv7l (AArch32), and ARMv8l (AArch64) glibc-based Linux, Windows 10/Server (1803+), and macOS 10.14+ (Mojave) SSH hosts.
* [Remote-Tunnels](https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-server) - Work with source code in any location by opening folders on a remote machine/VM using a VS Code Tunnel (rather than SSH).
* [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) - Work with a separate toolchain or container based application by opening any folder mounted into or inside a container.
* [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) - Get a Linux-powered development experience from the comfort of Windows by opening any folder in the Windows Subsystem for Linux.

**[Connect to a remote host](https://code.visualstudio.com/docs/remote/ssh#_connect-to-a-remote-host)** - follow these steps to connect to a remote host. 

## An Example C/C++ Development Environment using VS Code
* VS Code on Windows for code navigation and editing
* Optionally connected to a remote Linux machine via SSH
* 'Terminal' inside VS Code can be used for compilation/linking, or the JSON file can be configured for build targets/compiler-linker flags etc.
* Following extensions are highly recommended:
  * [C/C++ for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
  * [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
  * [vscodevim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)
* gdb running natively in a Linux terminal pointing to the binary for debug

> *⚠️ If you have a source file open in VS Code, but made an edit and saved it using Vim in another terminal, VS Code is quite responsive in applying those changes to the currently open file.*

## Navigating Code in VS Code
> *NOTE: Installing [vscodevim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) might change the default [keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) in VS Code.*

* **[Useful keyboard shortcuts:](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)**
  * ``Ctrl+,`` - to open and configure user settings for different VS Code
  * ``Ctrl+`` or ``Ctrl-`` - zoom in or zoom out
  * Go to line number within a file - ``Ctrl+G``
    * enter a line number, the line is previewed, hit ``ESC`` to cancel.
    * Indicates line numbers 1 thru max line number for the current file.
    * ``-1`` - negative offset takes you to the end of file.
* **Breadcrumbs** - VS Code can display the entire path from the source directory down to the symbol name. This can also be used to do source code navigation.
* **Outline View** - lists an outline of all symbols (functions, classes, structs, enums, typedefs, variables etc.) in the current file. Search and filter by symbol names. Set/adjust advanced settings for the outline view, as well as 'Follow Cursor', 'Sort by - Position, Name or Category' etc.
* **Quick Open** - ``Ctrl+P``
  * Open any file in the workspace by searching for a string, mutliple files with same name listed with a relative path.
  * Holding down `Ctrl` key opens files in a new editor group or tab.
  * Navigate with up/down arrow keys, or open a split view. To open multiple files - use Ctrl+➡️ key
  * Fuzzy filtering - search within a specific dir(i.e. <dir> <string>), camelcase search etc.
* **Command Palette** - ``Ctrl+Shift+P``
  * ``Go Back`` (``Alt+⬅️``) or ``Go Forward`` (``Alt+➡️``) command to go the previous or next location in navigation history
  * ``Go to Last Edit Location`` - go back to the last location where an edit was made
  * ``Go to Bracket`` - match brackets
  * ``Go to Symbol`` - navigate to a symbol (function name, class, struct, enum, typedef, variable etc.).
    * ``Ctrl+Shift+O`` to navigate through list of symbols in the current file.
    * ``@:`` sorts symbols by type. You can also combine it with the Quick Open by appending ``@`` to the file name.
    * ``Ctrl+T`` navigates through all symbols in the workspace.
  * ``Go to Definition`` (``F12``) - jump to the definition of the symbol
  * ``Go to Declaration`` - jump to the declaration of the symbol
  * ``Go to References`` - lists all references to the current symbol in a inline peek view box
  * ``Find all references`` - finds all references to the symbol, in a file explorer outline
  * ``Peek Definition`` (``Alt+F12``) - inline view of the definition
  * Call hierarchy
* ``code --diff <file1> <file2>`` - to diff two files using VS Code diff capability

## Editing Code in VS Code
## Formatting Code in VS Code