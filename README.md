Build
=====

Build using scripts
-------------------

In order to build a software with scripts, you just have to add build
instructions to a script. Writing a script is easy and you can do almost
any kind of processing. However it is not easy to make a good, portable,
customizable build processing. It is also difficult to properly implement
incremental building, that avoid rebuilding everything if not needed.
It is also difficult to exploit multiple processors, if available.

### Compile ###

```
./build.sh
```

### Clean ###

```
./build.sh clean
```

### Pros ###

* Easy to write
* Can do almost everything

### Cons ###

* Non standard
* No incremental build
* No build-time customization
* No parallel building

Build using Makefile
--------------------

`make` is a build tool that tries to automate software building.
Its main goals are reliability, incremental building and parallel
building. `make` achieves most of its goals by expressing each
build step (rule, in `make` terminology) in terms of preconditions
and expected results (targets, in `make` terminology).
Rules are written in files called `Makefile`.
Each rule is defined by a result target name, a list of prerequisite
target names, a sequence of commands that, given the prerequisites
allows to build the result. Each target is implicitly associated to
a file and each rule is assumed to contain instructions to create
a file whose name is the result target name. When `make` is instructed
to build a target, it will check if the file associated to the target
is already present. If it is not present it will then check if prerequisites
are satisfied (i.e., if, for each prerequisite target, a file exist whose name
is the same of the target one) and then proceed to run the commands in the rule.
If the result file is present it will check if prerequisites files are older.
If they are older, no action will be performed. If they are newer, `make`
will proceed to run the commands in the rule.

The typical rule is written as follow:

```
result: prerequisite1 prerequisite2
  command1 prerequisite1 prerequisite2 result
  command2 prerequisite1 prerequisite2 result
  ...
```
and a real-life example could be:

```
main: main.c
  cc main.c -o main
```
which defines the rule to build `main` executable, given
`main.c` source file.

It is possible to build a specific target by issuing `make $target`
command. If no target is specified, `make` will either try to
build the first defined target or a target named `all`, if present.

Rules in `Makefile` are very convenient to grant incremental and parallel
builds. Unfortunately the syntax of `Makefile` is sometime confusing and
limited: it is very hard (or impossible) to write a proper `Makefile`
when file names contain spaces, tabs are part of the syntax, the
rule order matters, and so on. It is possible to write `Makefile` for
almost any kind of build, as soon as input and output are separated
(i.e., if the build steps are not supposed to update their own input).
It is also possible to use environment variables to customize the build process at
build time.

### Compile ###

```
make -j
```

### Cross-compile with Mingw ###

```
make -j CC=i686-w64-mingw32-gcc AR=i686-w64-mingw32-ar
```

### Clean ###

```
make -j clean
```

### Pros ###

* Mostly standard
* Can do almost everything
* Incremental build
* Partial build-time customization
* Parallel building

### Cons ###

* Little integration with most IDE
* Confusing/limited/error-prone syntax
* Hard to reliably reproduce builds due to large use of environment variables

Build using CMake + Makefile
----------------------------

### Configure ###

```
mkdir -p build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=install  -DCMAKE_VERBOSE_MAKEFILE=ON
```


### Compile ###

```
make -j
```

### Install ###

```
make -j install
```

### Clean ###

```
make -j clean
```

Build using CMake + Ninja
----------------------------

### Configure ###

```
mkdir -p build-ninja
cd build-ninja
cmake .. -GNinja -DCMAKE_INSTALL_PREFIX=install
```


### Compile ###

```
ninja
```

### Install ###

```
ninja install
```

### Clean ###

```
ninja clean
```

Cross-build using CMake + Makefile
----------------------------------

### Configure ###

```
mkdir -p build-mingw32
cd build-mingw32
cmake .. -DCMAKE_INSTALL_PREFIX=install  -DCMAKE_VERBOSE_MAKEFILE=ON -DCMAKE_TOOLCHAIN_FILE=../toolchains/Toolchain-mingw32.cmake
```


### Compile ###

```
make -j
```

### Install ###

```
make -j install
```

### Clean ###

```
make -j clean
```

Build + Coverage test using CMake + Makefile
--------------------------------------------

### Configure ###

```
mkdir -p build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=install  -DCMAKE_VERBOSE_MAKEFILE=ON -DCMAKE_BUILD_TYPE=Debug
```


### Compile ###

```
make -j
```

### Install ###

```
make -j install
```

### Clean ###

```
make -j clean
```

### Test ###

```
make -j test
```

### Coverage ###

```
make -j coverage
```

Run
===

Run native programs
-------------------

```
./main
./main2
./main3
LD_LIBRARY_PATH=. ./main4
```

Run Windows programs on Linux
-----------------------------

```
wine ./main
wine ./main2
wine ./main3
wine ./main4
```
