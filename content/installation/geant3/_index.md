+++
title = "Installing Geant3"
menuTitle = "Geant3"
chapter = false
hidden = false
showhidden = true
weight = 2
+++

Geant3 with VMC requires [ROOT](https://root.cern.ch/).

Since version 2.0, Geant3 with VMC uses [CMake](https://cmake.org/) to configure a build system for compiling and installing the headers, libraries and Cmake configuration files. To install geant3:

1. First get the Geant3 source from the [Download page](/download/geant3). We will assume that the Geant3 package sits in a subdirectory
```
/mypath/geant3
```

2. Create build directory alongside our source directory
```
$ cd /mypath
$ mkdir geant3_build
$ ls
 geant3 geant3_build    
```

3. To configure the build, change into the build directory and run CMake:
```
$ cd /mypath/geant3_build 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/geant3_install /mypath/geant3
```
  - If ROOT environment was defined using <code>thisroot.{c}sh</code> script, there is no need to provide the path to its installation. Otherwise, they can be provided using <code>-DROOT_DIR</code> cmake option.

  - Since version 2.1, the Geant3 library is built by default in <code>RelWithDebInfo</code> build mode (Optimized build with debugging symbols). This default can be changed via the standard CMake option <code>CMAKE_BUILD_TYPE</code>. The other useful values are <br>
      - <code>Release</code> : Optimized build, no debugging symbols <br>
      - <code>Debug</code> : Debugging symbols, no optimization <br>

4. After the configuration has run, CMake will have generated Unix Makefiles for building Geant3. To run the build, simply execute make in the build directory:
```
$ make -jN
```
where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).

  - If you need more output to help resolve issues or simply for information, run make as
```
$ make -jN VERBOSE=1
```

5. Once the build has completed, you can install Geant3 to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
```
$ make install
```

<hr>

The instructions above apply to the installation since the version 2.0. For the installation of the previous versions (1.x) see [Installing geant3 - Older Versions](geant3-old)

