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
  - there is a special widget that act as a main container and it's **app.c**. this file contains 3 important functions:
    - `initialize()`: All initializations happen here.
    - `render()`: The main loop of the program when all top level events come here. and then we use function pointers to dispatch them to local widget.
      Actually events do not process here. if you look carefully, for each event we call a function: `callFunctions()` and we pass it as a __Node*__. actually we delegate event handling to every widget.
    - `cleanup()`: Free all the resource allocated for the application(including dynamically allocated memories, SDL specific memories, etc..)
  - If you need some widgets that is not exists yes, feel free to raise an issue.
- example
  - **main.c**: the `main()` function resides here. it just call other functions from `app.c`. also it pass __initWidgets()__ function to **app.c**.
    if you add any new widget in your application, you should put it's init() function here.
  - sample widgets: there are some widgets like __btnTest__, __lblTest__, __txtInputTest__, etc..
    you can see how they implemented if you need ideas about widget creation.

## How use derafsh in my own project?
1. put **lib/** directory in your project.
2. start designing your scene by creating different widgets. you should follow some rules:
  2.1. for every widget that you create, you should provide an init() function for it in it's header file.(look at `txtInputTest.h`)
  2.2. in equivalent `.c` file, you should implement that init function.
    2.2.1. inside this function, you should set properties of each widget.(most derafsh widgets have a structure that you need to initialize)
    2.2.2. you should register callback functions to handle events.(look at `txtInputTest.c`, if you need some ideas)
3. finally, call your init function in `initWidgets()` that you can find it in **main.c**.

## License
![License](https://img.shields.io/github/license/LinArcX/derafsh.svg)
