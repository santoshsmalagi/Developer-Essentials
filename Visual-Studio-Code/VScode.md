# Visual Studio Code
Visual Studio Code is a lightweight, modern, but powerful source code editor. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages and runtimes (such as C++, C#, Java, Python, PHP, Go, .NET).

> *The following tutorial focuses on setting up a VS Code C/C++ development environment on Windows, using the MinGW (GCC) toolchain. VS Code website provides turotials for setting up C/C++ evelopment environment for other platforms/compiler toolchains.*
 
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
VS Code Extensions allow developers to extend the capability of VS Code by adding languages, debuggers, and tools to support several development workflows. VS Code's rich extensibility model lets extension authors plug directly into the VS Code UI and contribute functionality through the same APIs used by VS Code. VS Code extensions can be installed directly using the [Visual Studio Code Marketplace](https://marketplace.visualstudio.com/VSCode). 

Some interesting VS Code Extensions for C/C++ development:
* [C/C++ for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
* [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
* [Peacock](https://marketplace.visualstudio.com/items?itemName=johnpapa.vscode-peacock)

## Visual Studio Code vs Vim?

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

## Creating a C/C++ Project in VS Code

## Debugging C/C++ Code in VS Code
## Navigating Code in VS Code
## [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)

## Hot Keys
* Ctrl+Shift+` - opens a VS Code terminal window

