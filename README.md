# Derafsh
A gui library written in C and SDL2

# Prerequisites
## libs
- SDL2_ttf-devel
- SDL-devel

## tools
- git
- gcc(or clang)
- gdb(or seergdb)
- fzf

# Build/Run/Clean
## Linux
First source **project.sh** file:
`. project.sh`

And then call `p`(it's a bash function).

Now you can see a list of commands, like:
- run
- clean
- debug
- generate tags
- valgrind
- find strings in the binary
- list symbols from object files

# Architecture
There are two main directories:
- lib
  - In this directory, you can find graphical widget(https://en.wikipedia.org/wiki/Graphical_widget).
  - If you need some widgets that is not exists yes, feel free to raise an issue.
- example
  And here we have two main important files:
    - main.c: the c main function. it just call other functions from app.c.
    - app.c: this file contains 3 important functions:
      - initialize(): All initializations happen here.
      - render(): The main loop of the program when all top level events come here. and then we use function pointers to dispatch them to local widget.
        Actually events do not process here. if you look carefully, for each event we call a function: `callFunctions()` and we pass it a Node*. we delegate event handling to every widget.
      - cleanup(): Free all the resource allocated for the application(including dynamically allocated memories, SDL specific memories, etc..)
  And some other files like: btnAdd.c, txtInputTest.c
    - When you want to create some widgets, you should create a new file for it and put all the logic in it(like initialization, event handling, etc..)

# How use derafsh in my own project?
1. put **lib/** directory in your project.
2. start designing your scene by creating different widgets. you should follow some rules:
  2.1. for every widget that you create, you should provide an init() function for it in it's header file.(look at `txtInputTest.h`)
  2.2. in equivalent `.c` file, you should implement that init function.
    2.2.1. inside this function, you should set properties of each widget.(most derafsh widgets have a structure that you need to initialize)
    2.2.2. you should register callback functions to handle events.(look at `txtInputTest.c`, if you need some ideas)
3. finally, call your init function in `initWidgets()` that you can find it in **main.c**.

## License
![License](https://img.shields.io/github/license/LinArcX/derafsh.svg)
