# Snap7 Communications Library
[Snap7](http://snap7.sourceforge.net/) is a library for communicating with a Siemens S7 PLC.

This repository contains a fork of the source code where we replaced the makefiles with a CMake build/install system, so that we can easily find and link with the installed library from other CMake projects.

The library consists of two parts: the actual library `Snap7`, and the wrapper library `Snap7Wrapper`. The originally intended method to link with Snap7 is to link with `libsnap7.so` and then build `snap7.cpp`, which contains the implementation symbols for the C++ class methods, alongside your client code. This is obviously not user friendly, so we compiled the C++ symbols into a separate library that can also be linked with.

To build the library:

```bash
# Run CMake
mkdir build
cd build
cmake ..

# Build the libraries
make
# Optionally, build the examples
make examples

# Install globally
sudo make install
```

To link your client application with the built library:

```cmake
find_package(Snap7 REQUIRED)
add_executable(myapp app.cpp app.h) # (for example)
target_link_libraries(myapp
  PUBLIC
  Snap7::Snap7        # Always
  Snap7::Snap7Wrapper # Only needed if using the c++ wrapper class
  )
```

Library and include directories will automatically be set by linking with the Snap7 targets imported by `find_package`.

## Original README.txt
Here follows the contents of the originally included README.txt.

```
Snap7 Package 1.4.2 (See History.txt for details)

The files deployed differs only for their compression method, their content is the same.

7-Zip is the best and today is natively supported by many OS.
Anyway, in doubt, download the .zip archive for Windows and .gz archive for Unix (Linux, BSD, Solaris).

There is an unique package for all the platforms since the source code (library, examples and wrappers) is fully multi-platform. Compiled examples, libraries deployed and project/makefiles are divided by folder.

No installation is needed, unpack snap7-full-x.y.z wherever you want : all paths inside projects/makefiles are relative, both for Windows and Unix.

The compiled examples and rich-demos are ready to run, in Unix remember to copy the correct libsnap7.so (release/deploy.html contains the list divided by OS/distro) in usr/lib.


Linux ARM boards
================

If you plan to download the package directly from one of these boards, you can safely delete, after unpacking, all folders relative to windows/bsd/solaris and i386/x86_64 Linux to have more room in your SD card.

As you can see in the online documentation, Snap7 was succesfully built and tested with 

- Raspberry PI     (ARM V6)
- Raspberry PI 2   (ARM V7)
- Raspberry PI 3   (ARM V7)
- pcDuino          (ARM V7)
- BeagleBone Black (ARM V7)
- CubieBoard 2     (ARM V7)
- UDOO Quad        (ARM V7)


I'm pretty sure that the libraries deployed can run in other same-class linux boards if they are "standard Linux" based, and very sure that the libraries can be succesfully built in the others using the correct makefile (V6 or V7).

Please report feedback and send the libraries if you do this.
```
