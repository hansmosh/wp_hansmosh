---
ID: 47
post_title: >
  Install libftdi for Python3.4 on Ubuntu
  14.04
author: hansmosh
post_date: 2015-09-16 09:55:29
post_excerpt: ""
layout: post
permalink: http://hansmosh.com/?p=47
published: false
---
As usual, Adafruit has a great guide but I use Python3.4 and it's not quite working for that.

https://learn.adafruit.com/adafruit-ft232h-breakout/linux-setup

tl:dr Update cmake and replace the `cmake` step with `cmake -DCMAKE_INSTALL_PREFIX="/usr/" -DPYTHON_EXECUTABLE="/usr/bin/python3.4" ../`

OK, but how did I figure that out?

1) What's wrong in the first place? When I run the original `cmake -DCMAKE_INSTALL_PREFIX="/usr/" ../`, two things are off: `-- Found PythonLibs: /usr/lib/x86_64-linux-gnu/libpython2.7.so (found version "2.7.6")
-- Found PythonInterp: /usr/bin/python (found version "2.7.6")` No errors, but we don't want to see Python2.7 anywhere. Everyhting installs fine but when you run `python3.4` and `import ftdi1`, it's not going to work

2) What's up with the Python interpreter? It's no surprise that `which python` returns `/usr/bin/python` since that's the default value that's being used. But we want `which python3.4` so that we use `/usr/bin/python3.4`.

3) But how do we tell cmake to use that one? The file `python/CMakeLists.txt` is where I figure our Python related cmake configuration is hiding so I open that up and see PYTHON_EXECUTABLE. Looks good so let's try it out: `cmake -DCMAKE_INSTALL_PREFIX="/usr/" -PYTHON_EXECUTABLE="/usr/bin/python3.4" ../`

4) Using cmake flags: Well, that didn't do anything. That other flag `DCMAKE_INSTALL_PREFIX` has a D at the front so what happens if we use that: `cmake -DCMAKE_INSTALL_PREFIX="/usr/" -DPYTHON_EXECUTABLE="/usr/bin/python3.4" ../`

`cmake` is running again and we're making progress: `-- Found PythonInterp: /usr/bin/python3.4 (found version "3.4")`

Note: as we're figuring this out, clear out the build directory before each `cmake` with `rm -rf ./*`

5) Use 3.4 for PythonLib. You may not have this issue, but my fix here is to update where cmake finds the python library. After much digging around, I have determined that the file `FindPythonLibs.cmake` needs to be updated to support Python3.4. Open up the file and scroll down to where the Python versions are defined. `sudo vim /usr/share/cmake-2.8/Modules/FindPythonLibs.cmake` This line needs to have `3.4` added to it: `set(_PYTHON3_VERSIONS 3.4 3.3 3.2 3.1 3.0)`

Now I can `cmake -DCMAKE_INSTALL_PREFIX="/usr/" -DPYTHON_EXECUTABLE="/usr/bin/python3.4" ../`, `make` and `sudo make install` and everything is looking good.