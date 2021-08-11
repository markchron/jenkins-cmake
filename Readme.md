
A Git/Fortran/CMake/Jenkins example 

using Fortran, CMake to generate a project

# CMake
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
```

# Git

## Create a new repository on the command line
```
## change branch name
#$ git branch -m main 
# add remote repository
$ git remote add origin git@github.com:markchron/jenkins-cmake.git
#
$ git push -u origin master
```

## Push an existing repository from the command line
```
# link repository 
$ git remote add origin git@github.com:markchron/jenkins-cmake.git
#$ git branch -m main
# Push code into repository
$ git push -u origin master
```

# Troubleshoot
> git@github.com: Permission denied (publickey).

Check if our key is being used to make an SSH connection, by cmd 
`eval "$(ssh-agent -s)" ssh-add -l -E md5`
The first output shows the SSH agent PID on your computer. 
Then lists all the SSH keys that are configured on your machine. 

Check to see if they match the one you've uploaded to GitHub. 

Generate an SSH key `ssh-keygen -t rsa -b 4096 -C "email@email.com" ssh-add -K ~/.ssh/id_rsa`


