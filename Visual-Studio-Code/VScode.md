# Visual Studio Code
Visual Studio Code is a lightweight, modern, but powerful source code editor. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages and runtimes (such as C++, C#, Java, Python, PHP, Go, .NET).

> *The following tutorial focuses on setting up a VS Code C/C++ development environment on Windows using the MinGW (GCC) toolchain. VS Code website provides turotials for setting up C/C++ evelopment environment for other platforms/compiler toolchains.*
 
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
* Vim *(or Emacs)* is de-facto standard for text editing on the Linux terminal, especially if a GUI environment cannot be loaded
* Code navigation in Vim using [Exuberant ctags](https://ctags.sourceforge.net/) requires regeneration of a tag file, everytime new code is checkout out. VS Code automatically updates the list of function declarations/definitions if new code was checked out.
* Differences in keyboard shortcuts/hot-key bindings between VS Code vs Vim. It is possible to emulate Vim *(or Emacs)* keyboard shortcuts in your VS Code environment. For example, using the [vscodevim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) extension, key bindings can be emulated to replicate Vim (though not all Vim features may be supported).
* VS Code by default list a source directory tree for easy navigation and auto-populates it everytime new code is checked out. Achieving similar functionality in Vim might require additional plugins/extra work.
* Another notable feature of VS Code is it's ability to list an 'Outline' view of the current C/C++ source file or header file and list globals, function names, typedefs, structs, enums etc.
* Opening a terminal inside of Vim and running the debugger seemed more intutive in Vim vs VS Code. Even better running gdb natively on Linux offered superior performance and control over running gdb inside of VS Code (personal opinion).
* Syntax highlighing and semantic colorization in Vim for some specialized languages e.g. Tcl-Tk, Verilog etc. seemed superior to VS Code.
* VS Code offers some powerful extensions for version control systems such as [Perforce for VS Code](https://marketplace.visualstudio.com/items?itemName=mjcrouch.perforce)

## Configuring Visual Studio Code for C/C++ Development

#### Step I - [Download and Install VS Code](https://code.visualstudio.com/docs/setup/setup-overview)
Provides step by step instruction for Windows, Linux, and macOS to download and install VS Code.

#### Step II - [C/C++ Extension for VS Code](https://code.visualstudio.com/docs/languages/cpp)
The next step is to enable C/C++ development support for VS Code by installing the [Microsoft C/C++ Extension](https://code.visualstudio.com/docs/languages/cpp) in VS Code. This extension is a *must have* for C/C++ development in VS Code and adds useful features such as syntax highlighting (colorization), smart completions and hovers (IntelliSense), error checking etc.

> *The C/C++ extension doesn't include a C++ compiler or debugger.*

#### Step III - [Setup C/C++ Development Tools](https://code.visualstudio.com/docs/languages/cpp)
VS Code as an editor relies on command-line tools for C/C++ development workflow. The compiler-linker-debugger toolchain must therefore be installed, or VS Code must be configured to use these tools if they are already installed.

Open a VS Code terminal window by Ctrl+Shift+`. USe the following command to check if GCC has been installed on the system:
```bash
g++ --version
```

**[Installing the MinGW-w64 toolchain](https://code.visualstudio.com/docs/cpp/config-mingw#_prerequisites)**  
The latest version of MinGW-w64 can be installed on Windows via MSYS2, which provides up-to-date native builds of GCC, MinGW-w64, and other helpful C++ tools and libraries to compile code, debug it, and configure it to work with IntelliSense.

> *[MSYS2](https://www.msys2.org/) is a collection of tools and libraries providing you with an easy-to-use environment for building, installing and running native Windows software. MSYS2 provides up-to-date native builds for GCC, mingw-w64, CPython, CMake, Meson, OpenSSL, FFmpeg, Rust, Ruby, just to name a few.*

#### Step IV (optional) - [Remote Development Setup](https://code.visualstudio.com/docs/remote/remote-overview)
The [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension pack allows you to open any folder in a container, on a remote machine, or in the Windows Subsystem for Linux (WSL) and take advantage of VS Code's full feature set. No source code needs to be on your local machine to gain these benefits since Remote Development runs commands and extensions directly on the remote machine.  

This Remote Development extension pack includes four extensions:
* [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) - Work with source code in any location by opening folders on a remote machine/VM using SSH. Supports x86_64, ARMv7l (AArch32), and ARMv8l (AArch64) glibc-based Linux, Windows 10/Server (1803+), and macOS 10.14+ (Mojave) SSH hosts.
* [Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-server) - Tunnels - Work with source code in any location by opening folders on a remote machine/VM using a VS Code Tunnel (rather than SSH).
* [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) - Work with a separate toolchain or container based application by opening any folder mounted into or inside a container.
* [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) - Get a Linux-powered development experience from the comfort of Windows by opening any folder in the Windows Subsystem for Linux.

**[Connect to a remote host](https://code.visualstudio.com/docs/remote/ssh#_connect-to-a-remote-host)** - follow these steps to connect to a remote host. 

## VS Code Shortcuts/Tips and Tricks
* Ctrl+Shift+` - opens a VS Code terminal window
* ``code --diff <file1> <file2>`` - to diff two files using VS Code diff capability

## Creating a C/C++ Project in VS Code
## Debugging C/C++ Code in VS Code
## Navigating Code in VS Code
