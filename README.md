About
-----

OficinaFramework is a multiplatform framework for game development, created by Lucas Vieira.
It is focused on bringing a layer of accessibility for modern OpenGL games, using C++ as
language. While it makes a game developer's life easier, it still brings about a lot of
support for advanced system features which other languages and engines insist on hiding.
This way, the programmer can tweak the game's performance without a heavyweight system.


License
-------

This engine is distributed under the LGPL v3.0 license.
You can read more about it [here](http://choosealicense.com/licenses/lgpl-3.0).

Documentation
-------------

You can either access or download Oficina's documentation:

- [View Oficina's Documentation online (v2.0.11)](https://luksamuk.github.io/oficina/doc)
- [View Oficina's Documentation offline (v2.0.11 - PDF format)](https://luksamuk.github.io/oficina/oficina2.pdf)
- [View Oficina's Documentation online (v1.3)](luksamuk.github.io/OficinaFramework)

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
- (Optional) GNU Guile >= 2.0


Build
-----

Just `cd` to the repo's folder and use CMAKE. This will create a static library. You'll then be able to install it to your path.

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

Build (Without GNU Guile)
-------------------------

There's a possibility that you don't want to use Scheme, or you don't have GNU Guile installed in your system
(specially on Windows, which, by the time of this writing, has no proper port of GNU Guile - some of the ports
will make your application crash on startup, due to lack of stable multithread support).

Given that, you might want to disable the IronScheme support.
Follow the following procedures:

```bash
mkdir build
cd build
cmake .. -DNO_SCHEME=on
make
sudo make install
```

Or just define `-DNO_SCHEME` as `on` on your CMake GUI, before building.


Copyright
---------

(c) 2017 Lucas Vieira
