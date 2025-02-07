+++
title = "Installing Geant3"
menuTitle = "Geant3"
chapter = false
hidden = false
showhidden = true
weight = 3
+++

Geant3 with VMC requires the [VMC core package](/user-guide/vmc/vmc-library) and [ROOT](https://root.cern.ch/).

The [vmc core package](/user_guide/vmc/vmc-library) was separated from the ROOT source into a new stand-alone [vmc](https://github.com/vmc-project/vmc) package in the GitHub [vmc-project](https://github.com/vmc-project) organization. The motivation for this step was a gain in flexibility and faster workflow for new developments of multiple engine mode. The `vmc` package in ROOT is deprecated since ROOT version 6.18 (its compilation is optional) and it is going to be removed in the next ROOT version, 6.26. The VMC stand-alone is supported since Geant3 3.0.

Geant3 with VMC uses [CMake](https://cmake.org/) to configure a build system for compiling and installing the headers, libraries and Cmake configuration files. 

To install geant3:

1. First get the Geant3 source from the [Download page](/download/git-geant3). We will assume that the Geant3 package sits in a subdirectory
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
  - If ROOT environment was defined using `thisroot.{c}sh` script, there is no need to provide the path to its installation. Otherwise, they can be provided using `-DROOT_DIR` cmake option.

  - Since Geant3 3.9, the VMC stand-alone library has to be provided using `-DVMC_DIR` cmake option.  If the VMC stand-alone library is not found, the deprecated VMC library in ROOT (availble if ROOT was built with the `vmc` option enabled) can still be used, a deprecation warning will be issued in this case.

  - Since Geant3 4.0, **the deprecated VMC library in ROOT cannot be used** and building against ROOT built with the `vmc` option enabled will fail with CMake error.

  - The Geant3 library is built by default in `RelWithDebInfo` build mode (Optimized build with debugging symbols). This default can be changed via the standard CMake option `CMAKE_BUILD_TYPE`. The other useful values are <br>
      - `Release` : Optimized build, no debugging symbols <br>
      - `Debug` : Debugging symbols, no optimization <br>

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

