About
-----

OficinaFramework is a multiplatform framework for game development, created by Lucas Vieira.
It is focused on bringing a layer of accessibility for modern OpenGL games, using C++ as
main language, and Guile Scheme and Lua as scripting languages. While it makes a game developer's life easier, it still brings about a lot of
support for advanced system features which other languages and engines insist on hiding.
This way, the programmer can tweak the game's performance without a heavyweight system.


License
-------

This engine is distributed under the LGPL v3.0 license.
You can read more about it [here](http://choosealicense.com/licenses/lgpl-3.0).

Documentation
-------------

You can either access or download Oficina's documentation:

- [View Oficina's Documentation online (v2.0.12)](https://luksamuk.github.io/oficina/doc)
- [View Oficina's Documentation offline (v2.0.12 - PDF format)](https://luksamuk.github.io/oficina/oficina2.pdf)
- [View Oficina's Documentation online (v1.3)](https://luksamuk.github.io/OficinaFramework)

Download
--------

Coming soon.

Dependencies
------------

- SDL2 >= 2.0.5
- SDL2\_Image >= 2.0.0
- OpenGL 3.3 support or higher
- GLEW >= 2.0.0
- GL Mathematics (GLM) >= 0.9.8
- (Optional) GNU Guile >= 2.0.11
- (Optional) Lua >= 5.3


Building Your Own Projects With Oficina2
----------------------------------------

Building your own game, which uses Oficina2, is made really easy by using the program `oficina2-config`, which is provided when building Oficina.
This program offers three arguments, to be used when invoked:

- `--version`: Shows Oficina's build version;
- `--cppflags`: Shows the flags needed to build a C++ game or application based on Oficina;
- `--libs`: Shows the flags to link libraries on a C++ game or application based on Oficina.

Suppose you created a file called `game.cpp` which holds the code for your game. To build your oficina2 app with GCC. under Linux, simply use:

```g++ game.cpp `oficina2-config --cppflags` `oficina2-config --libs` -o MyGameBinary ```

If you're using Windows, you may want to simply catch the output of commands `oficina2-config --cppflags` and `oficina2-config --libs` and
compile your program in that order.

This will also work with LLVM Clang, and will respect the libraries which you build Oficina2 with, so it's very important to build
the `oficina2-config` target on CMake along with Oficina2's static library and test app.


## Building

### Building on Linux

Just `cd` to the repo's folder and use CMAKE. This will create a static library. You'll then be able to install it to your path.

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

#### Building Without GNU Guile or Lua

##### Without Guile

There's a possibility that you don't want to use Scheme, or you don't have GNU Guile installed in your system
(specially on Windows, which, by the time of this writing, has no proper port of GNU Guile - some of the ports
will make your application crash on startup, due to lack of stable multithread support).

Given that, you might want to disable the IronScheme support.
When running CMake, use this command:

```cmake .. -DNO_SCHEME=on```

##### Without Lua

There's also a possibility that you don't want to use Lua, with no scripting nor REPL language at all.
In that case, you should run CMake, when building, as the following command:

```cmake .. -DNO_LUA=on```

That will disable building and linking of Lua libraries for Oficina.

##### Without Both

If you want to disable both Scheme and Lua support, just combine the two commands into one:

```cmake .. -DNO_SCHEME=on -DNO_LUA=on```



### Building on Windows

Building Oficina2 on Windows is easy, but is not as intuitive as with a Unix system.

Currently, Oficina2 supports MinGW (32-bit) and, to compile it with CMake, you'll need to make sure you:

- Have the proper library dependencies installed on your MinGW path;
- Have `mingw32-make` installed on your MinGW installation (you can easily select this package on mingw-get);
- Make sure you have installed Git without `cmd` support, only with Git Bash (or CMake will give an error regarding Git's own `sh`).

Because of the last situation, you also probably won't be able to compile using a Git Bash or Cygwin.

After that, open the Command Prompt, `cd` to the folder you have cloned this repository, then build the program:

```cmd
mkdir build
cd build
cmake -G "MinGW Makefiles" ..
make
```

If you're running the Command Prompt in Administrator Mode, you can use `make install` to directly install
OficinaFramework in the folder `C:\Program Files (x86)\OficinaFramework2`.

For selectively building with or without Lua, please refer to the Linux section.

Notice that, on Windows systems, IronScheme support is DISABLED BY DEFAULT. This is due to the current
GNU Guile version (2.0.14, by the time of this writing), which does not work well with Windows.

It is also advisable to add the directory `C:\Program Files (x86)\OficinaFramework2\bin` to your PATH so you can use Oficina2's config tool and test tool.



Learn
-----
You can access Oficina2 documentation online on https://luksamuk.github.io/oficina or explore the `examples`
folder, which is didatically commented so you can learn the basics.

Be mindful that these libraries are still in development, so some intended features are still lacking. The
same applies for tutorials and documentations, which hopefully will be delivered soon.


Copyright
---------

(c) 2017 Lucas Vieira
