# Building GCC and libstdc++ from Source

This tutorial walks through compiling the GNU Compiler Collection (GCC) from source on Linux, including the GNU C++ Standard Library (libstdc++), and installing the result alongside your system compiler without disturbing it.

> [!CAUTION]
> ## ⚠️ Building libstdc++ separately from the rest of GCC is not supported!!!
> glibc is a separate project (sourceware.org/glibc) with its own configure and make, and it doesn't need GCC's source tree.
> libstdc++ lives inside the GCC source tree (in `libstdc++-v3/`) and is built automatically whenever the C++ front end is enabled. If you need a newer libstdc++, the way to get it is to build GCC. **libstdc++ can't be built without GCC because it's tied to the compiler (for e.g. GCC has libstdc++ and LLVM has libc++)**. It implements exceptions, runtime type information and operator new, its headers use compiler built-ins, and new C++ features arrive in the compiler and library together.
> glibc is tied to the kernel and the OS instead. **What you can't safely do is replace the system's glibc.**

**Reference:** [GCC Installation Guide](https://gcc.gnu.org/install/)

---

## Overview

The build follows the usual GNU pattern: make sure the host has the right tools, fetch the source, configure in a separate build directory, compile, optionally run the test suites, and install into a directory of your own.

| Step | Purpose | Key command |
|------|---------|-------------|
| 0. Prerequisites | Make sure the host has the tools and libraries GCC needs | `./contrib/download_prerequisites` |
| 1. Download | Get the GCC source as a release tarball or from Git | `wget …` or `git clone …` |
| 2. Configure | Generate Makefiles in a separate build directory | `../srcdir/configure …` |
| 3. Build | Compile the compiler and its runtime libraries, including libstdc++ | `make -j"$(nproc)"` |
| 4. Test *(optional)* | Run the DejaGnu test suites to catch problems before installing | `make -k check` |
| 5. Install | Copy everything into a dedicated install prefix | `make install` |

### Conventions

Following the GCC documentation, this tutorial uses two names throughout. **srcdir** is the top-level source directory (the one containing `configure`), and **objdir** is the top-level build, or object, directory where you run `configure` and `make`. Keep them as siblings: the GCC docs strongly recommend that the build directory not live inside the source tree.

```text
~/GCC_16.2.0/
├── srcdir/    # GCC source tree
└── objdir/    # configure, make, and make check all run here
```

The commands below use a few shell variables so you only need to change the version in one place. GCC 16.2.0 is used as the example; any release listed at <https://ftp.gnu.org/gnu/gcc/> works the same way.

```bash
export GCC_VERSION=16.2.0
export GCC_ROOT="$HOME/GCC_${GCC_VERSION}"        # holds srcdir/ and objdir/
export GCC_PREFIX="$HOME/opt/gcc-${GCC_VERSION}"  # where the finished compiler is installed
```

Budget several gigabytes of free disk space and a fair amount of time. A full native build can take anywhere from tens of minutes to a few hours, depending on how many CPU cores you have.

---

## Step 0: Prerequisites

GCC requires a number of tools and packages to be available on the host system. At a bare minimum you need the following.

| Requirement | What it's for | Notes |
|-------------|---------------|-------|
| An existing C++ compiler | GCC is written in C++, so it has to be bootstrapped with a compiler you already have | GCC 15 and later need a C++14 compiler (GCC 5.4 or newer). Releases before GCC 15 only need C++11 (GCC 4.8.3 or newer). |
| C standard library and headers (glibc) | The main interface between programs and the kernel | Must be present for every target variant being built. See the multilib note in Step 2. |
| libgcc | Low-level runtime routines for operations the target CPU can't perform directly in a single instruction | Comes with your existing host GCC. A new libgcc is also built as part of this build. |
| GNU Binutils | The assembler (`as`), linker (`ld`), and related binary utilities | Version 2.35 or newer is required if you use link-time optimisation (LTO). |
| GNU make 3.80+ | Drives the build | |
| A POSIX shell (bash), awk, tar, gzip/xz, bzip2 | Running `configure` and unpacking archives | `zsh` does not work with GCC's `configure`. |
| GNU flex | Generates the lexical analyser | Essential when building from Git, because generated files aren't stored in the repository. |
| Perl 5.6.1+ | Used by parts of the build, including libstdc++ symbol versioning on some targets | |
| GMP, MPFR, MPC | Arbitrary-precision math libraries used inside the compiler | Fetched automatically by `download_prerequisites` (see Step 1). |
| isl *(optional)* | Enables the Graphite loop optimisations | Also fetched by `download_prerequisites`. |

On Debian or Ubuntu, the host tools can be installed with:

```bash
sudo apt install build-essential flex perl wget bzip2 xz-utils git
```

On Fedora, RHEL, or similar:

```bash
sudo dnf install gcc gcc-c++ make flex perl wget bzip2 xz tar git
```

---

## Step 1: Download the Source

GCC sources are available as release tarballs from the GNU FTP site and its mirrors, or directly from the GCC Git repository. Pick one of the two options below. Both end with the source in `srcdir/` and an empty `objdir/` next to it.

### Option A: Release tarball (recommended for most users)

<https://ftp.gnu.org/gnu/gcc/> stores the tarballs for every GCC release, and a list of mirrors is available at <https://gcc.gnu.org/mirrors.html>.

```bash
mkdir -p "$GCC_ROOT" && cd "$GCC_ROOT"
wget "https://ftp.gnu.org/gnu/gcc/gcc-${GCC_VERSION}/gcc-${GCC_VERSION}.tar.xz"
tar xf "gcc-${GCC_VERSION}.tar.xz"
mv "gcc-${GCC_VERSION}" srcdir
mkdir objdir
```

### Option B: Git

```bash
mkdir -p "$GCC_ROOT" && cd "$GCC_ROOT"
git clone git://gcc.gnu.org/git/gcc.git srcdir
cd srcdir
git checkout "releases/gcc-${GCC_VERSION}"   # pin to a release tag; skip to build the development trunk
cd ..
mkdir objdir
```

If your network blocks the `git://` protocol, use `https://gcc.gnu.org/git/gcc.git` instead. The full history is large, so for a quicker download of a single release you can add `--depth 1 --branch "releases/gcc-${GCC_VERSION}"` to the clone command.

### Download the remaining prerequisites

With the source in place, run the helper script from the top of the source tree. It downloads the GMP, MPFR, MPC, and isl sources into `srcdir` so they are built together with GCC, which avoids version mismatches with whatever your distribution ships. The script needs internet access.

```bash
cd "$GCC_ROOT/srcdir"
./contrib/download_prerequisites
```

If you'd rather use your distribution's packages (for example `libgmp-dev libmpfr-dev libmpc-dev libisl-dev` on Debian/Ubuntu, or `gmp-devel mpfr-devel libmpc-devel isl-devel` on Fedora), install those instead and skip this script.

---

## Step 2: Configure

GCC must be configured before it can be built, whether you are building a native compiler (as in this tutorial) or a cross compiler. Configuration always happens from inside `objdir`, pointing back at the `configure` script in `srcdir`.

> **Warning:** Unless it is absolutely needed, do **not** replace the system-provided compiler and runtime libraries in `/usr` or `/usr/local`. Other software on your system was built against them. GCC's default install prefix is `/usr/local`, so always pass `--prefix` explicitly.

```bash
cd "$GCC_ROOT/objdir"
../srcdir/configure -v \
    --prefix="$GCC_PREFIX" \
    --disable-multilib \
    --enable-languages=c,c++
```

| Option | Meaning |
|--------|---------|
| `-v` | Verbose output from `configure`. |
| `--prefix=DIR` | Where `make install` will put the finished compiler. Use a fresh directory that is separate from the system default. |
| `--disable-multilib` | Build a 64-bit-only compiler. On x86_64 the default is to also build 32-bit libraries, which requires the 32-bit glibc development headers. Without them the build fails with `fatal error: gnu/stubs-32.h: No such file or directory`. |
| `--enable-languages=c,c++` | Build only the C and C++ front ends. Enabling `c++` is what causes libstdc++ to be built. |

A few other options are worth knowing about:

| Option | When to use it |
|--------|----------------|
| `--program-suffix=-16` | Installs the binaries as `gcc-16`, `g++-16`, and so on, so they can't be confused with the system compiler. |
| `--disable-bootstrap` | Skips the three-stage bootstrap (see Step 3) for a much faster build, at the cost of the self-consistency check. Handy for experiments when your host compiler is recent. |
| `--with-gmp=DIR`, `--with-mpfr=DIR`, `--with-mpc=DIR` | Point at prerequisite libraries installed in a non-standard location. |

Run `../srcdir/configure --help` for the full list, or see <https://gcc.gnu.org/install/configure.html>.

**How to tell it worked:** `objdir` should now contain a `Makefile`, `config.status`, and `config.log`. If `configure` stopped with an error, the end of `config.log` usually explains why. If you change configure options later, start again from an empty `objdir` rather than reconfiguring on top of an old one.

---

## Step 3: Build

Once configuration succeeds, start the build with `make`. Running multiple jobs in parallel speeds it up considerably. `$(nproc)` uses one job per CPU core; a fixed value such as `-j 8` is fine too.

```bash
cd "$GCC_ROOT/objdir"
make -j"$(nproc)" 2>&1 | tee build.log
```

For a native build, `make` performs a **three-stage bootstrap** by default. Stage 1 compiles GCC with your existing host compiler, stage 2 recompiles GCC using the stage 1 compiler, and stage 3 does it once more with the stage 2 compiler. The stage 2 and stage 3 object files are then compared, and if they differ the build stops, which catches miscompilation. Finally, the runtime libraries (libgcc, libstdc++, libgomp, and others) are built with the finished compiler.

The freshly built libstdc++ ends up under `objdir/<target-triplet>/libstdc++-v3/`, for example `objdir/x86_64-pc-linux-gnu/libstdc++-v3/`. You can print your triplet with `../srcdir/config.guess`.

**If the build fails:** with many parallel jobs, the real error message is often buried above unrelated output. Search `build.log` for `error:`, or run `make` again without `-j` so the first failure is the last thing printed. If the machine runs out of memory, reduce the number of jobs.

---

## Step 4: Test (optional)

Running the test suites before installing gives you confidence in the new compiler and flags problems early. This step is optional and can take a long time.

The test suites are driven by **DejaGnu**, which in turn needs **Expect** and **Tcl**:

```bash
sudo apt install dejagnu     # Debian/Ubuntu (pulls in expect and tcl)
sudo dnf install dejagnu     # Fedora/RHEL
```

If `runtest` and `expect` are not on your `PATH` (for example, because you installed DejaGnu by hand under `/usr/local`), tell the test harness where to find their support files. Adjust these paths to match your installation; when DejaGnu comes from your distribution's package manager you normally don't need them at all.

```bash
export TCL_LIBRARY=/usr/local/share/tcl8.0
export DEJAGNULIBS=/usr/local/share/dejagnu
```

Run the full test suite from `objdir`. The `-k` flag tells `make` to keep going after failures, so you get a complete picture:

```bash
cd "$GCC_ROOT/objdir"
make -k -j"$(nproc)" check
```

To test only the pieces this tutorial focuses on:

```bash
# libstdc++ only (from objdir)
make -j"$(nproc)" check-target-libstdc++-v3

# C++ front end only (from the gcc/ subdirectory of objdir)
cd gcc && make -j"$(nproc)" check-c++
```

Results are written to `.sum` (summary) and `.log` (detailed) files, for example `objdir/gcc/testsuite/g++/g++.sum` and `objdir/x86_64-pc-linux-gnu/libstdc++-v3/testsuite/libstdc++.sum`. The script `../srcdir/contrib/test_summary` gathers all of them into a single report. DejaGnu warnings about not finding a global config file or tool init file are harmless. A handful of unexpected failures is normal on many platforms, and you can compare your results against those others have posted to the [gcc-testresults mailing list](https://gcc.gnu.org/pipermail/gcc-testresults/).

---

## Step 5: Install

Install into the prefix you chose in Step 2. Because it's a new directory under your home, no root privileges are needed. If you chose a system location such as `/opt`, run this with `sudo`.

```bash
cd "$GCC_ROOT/objdir"
make install
```

The install prefix will look roughly like this:

```text
$GCC_PREFIX/
├── bin/                            gcc, g++, cpp, gcov, ...
├── include/c++/16.2.0/             libstdc++ headers
├── lib64/                          libstdc++.so*, libgcc_s.so*, ...  (lib/ on some systems)
├── lib/gcc/<triplet>/16.2.0/       compiler-internal headers and libraries
└── libexec/gcc/<triplet>/16.2.0/   cc1, cc1plus, ...  (the actual compiler programs)
```

Once installed, `srcdir` and `objdir` can be deleted to reclaim disk space, although keeping `objdir` lets you re-run the tests later.

---

## Using Your New Compiler

Put the new compiler first on your `PATH` and confirm it's the one being picked up:

```bash
export PATH="$GCC_PREFIX/bin:$PATH"
which g++          # should print $GCC_PREFIX/bin/g++
g++ --version      # should report 16.2.0
```

Add the `export` line to your `~/.bashrc` if you want it to persist across sessions.

Then try a small program that uses a recent library feature:

```cpp
// hello.cpp
#include <print>

int main() {
    std::println("Hello from GCC {}.{}.{}", __GNUC__, __GNUC_MINOR__, __GNUC_PATCHLEVEL__);
}
```

```bash
g++ -std=c++23 hello.cpp -o hello
./hello
```

### The "GLIBCXX not found" gotcha

The program compiles against your **new** libstdc++ headers, but at run time the dynamic loader searches the system library directories and may load the system's **older** `libstdc++.so.6`. When that happens you'll see an error like this:

```text
./hello: /lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.xx' not found
```

There are three common fixes. You can embed the library path in the executable with an rpath:

```bash
g++ -std=c++23 hello.cpp -o hello -Wl,-rpath,"$GCC_PREFIX/lib64"
```

You can add the library directory to the loader's search path in your environment:

```bash
export LD_LIBRARY_PATH="$GCC_PREFIX/lib64${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}"
```

Or you can link the C++ runtime statically, so the binary doesn't depend on a shared libstdc++ at all:

```bash
g++ -std=c++23 hello.cpp -o hello -static-libstdc++ -static-libgcc
```

To check which libstdc++ a binary will actually load, run `ldd ./hello | grep libstdc++`.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `fatal error: gnu/stubs-32.h: No such file or directory` | Multilib build without 32-bit glibc headers | Reconfigure with `--disable-multilib`, or install your distribution's 32-bit libc development package. |
| `configure: error: Building GCC requires GMP …, MPFR … and MPC …` | Math prerequisite libraries are missing | Run `./contrib/download_prerequisites` in `srcdir`, or install the `-dev`/`-devel` packages. |
| `configure` warns about an invalid host type, or `make` reports `No rule to make target '–j'` | A hyphen was pasted as a typographic en dash (`–`), which often happens when copying commands from slides or word processors | Retype the option with a plain ASCII hyphen (`-v`, `-j`). |
| `configure: error: unrecognized option` | A misspelled option, such as `--enablelanguages` | Check the spelling; the correct form is `--enable-languages`. |
| A Git checkout fails to build because flex is missing | flex isn't installed, and generated lexer files aren't stored in the repository | Install flex and run `make` again. |
| `GLIBCXX_3.4.xx not found` when running a program | The system's older libstdc++ is being loaded at run time | Use an rpath, `LD_LIBRARY_PATH`, or static linking, as shown above. |
| The build is killed or the machine becomes unresponsive | Too many parallel jobs for the available memory | Lower the `-j` value. |
| Changed configure options don't seem to take effect | Stale files left in `objdir` | Delete `objdir`, recreate it empty, and configure again. |

---

## Quick Reference

The whole process as a single script, using the tarball download and the options from this tutorial:

```bash
#!/usr/bin/env bash
set -euo pipefail

export GCC_VERSION=16.2.0
export GCC_ROOT="$HOME/GCC_${GCC_VERSION}"
export GCC_PREFIX="$HOME/opt/gcc-${GCC_VERSION}"

# Step 1: download the source and set up srcdir/objdir
mkdir -p "$GCC_ROOT" && cd "$GCC_ROOT"
wget "https://ftp.gnu.org/gnu/gcc/gcc-${GCC_VERSION}/gcc-${GCC_VERSION}.tar.xz"
tar xf "gcc-${GCC_VERSION}.tar.xz"
mv "gcc-${GCC_VERSION}" srcdir
mkdir objdir

# Download missing prerequisites (GMP, MPFR, MPC, isl)
cd srcdir
./contrib/download_prerequisites

# Step 2: configure
cd ../objdir
../srcdir/configure -v \
    --prefix="$GCC_PREFIX" \
    --disable-multilib \
    --enable-languages=c,c++

# Step 3: build
make -j"$(nproc)"

# Step 4: test (optional; requires DejaGnu). Some failures are normal, so don't abort here.
make -k -j"$(nproc)" check || true

# Step 5: install
make install

echo "Done. Add $GCC_PREFIX/bin to your PATH to use the new compiler."
```

---

## Further Reading

The official GCC installation guide covers each step in depth: [prerequisites](https://gcc.gnu.org/install/prerequisites.html), [downloading](https://gcc.gnu.org/install/download.html), [configuration](https://gcc.gnu.org/install/configure.html), [building](https://gcc.gnu.org/install/build.html), [testing](https://gcc.gnu.org/install/test.html), and [final installation](https://gcc.gnu.org/install/finalinstall.html). For libstdc++ specifically, see the [libstdc++ manual](https://gcc.gnu.org/onlinedocs/libstdc++/).
