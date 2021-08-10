
A simple example using Fortran, CMake to generate a project

CMake generates a lot of files, so it is recommended that you build the program in a separate directory. 
On Linux and Unix, CMake defaults to creating a traditional __Makefile__, 
but CMake can also produce Ninja files to speed-up compilation (but see note below).
```
$ mkdir build
$ cd build
build $ cmake ..
build $ make
```

To take advantage of the Ninja build system, you must install Kitware’s Ninja branch 
which contains enhancements needed to compile Fortran programs. The -GNinja flag instructs CMake to generate Ninja files:
```
build $ cmake -GNinja ..
build $ ninja
```
You can use -D flags to override internal variables. For example:
```
build $ cmake -GNinja -DCMAKE_BUILD_TYPE=Debug -DCMAKE_Fortran_COMPILER=ifort -DCMAKE_INSTALL_PREFIX=/usr/local ..
build $ ninja

