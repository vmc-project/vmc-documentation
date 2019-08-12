+++
title = "Installing Geant4 VMC"
menuTitle = "Geant4 VMC"
chapter = false
weight = 3
+++

Geant4 VMC requires [ROOT](https://root.cern.ch/) and [Geant4](http://geant4.web.cern.ch/) installed, and optionally, it can be built with [VGM](https://github.com/vmc-project/vgm). See below tips for configuration and installation of these packages.

Since version 3.00, Geant4 VMC uses [CMake](https://cmake.org/) to configure a build system for compiling and installing the headers, libraries and Cmake configuration files. To install geant4_vmc:

1. First get the Geant4 VMC source from the  [Download page](/download/geant4_vmc). We will assume that the Geant4 VMC package sits in a subdirectory
```
/mypath/geant4_vmc
```
2. Create build directory alongside our source directory
```
$ cd /mypath
$ mkdir geant4_vmc_build
$ ls
 geant4_vmc geant4_vmc_build    
```

3. To configure the build, change into the build directory and run CMake:
```
$ cd /mypath/geant4_vmc_build 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/geant4_vmc_install /mypath/geant4_vmc
```

  - If ROOT and Geant4 environment was defined using `thisroot.[c]sh` and `geant4.[c]sh` scripts, there is no need to provide path to their installations. Otherwise, they can be provided using `-DROOT_DIR` and `-DGeant4_DIR` cmake options.

4. After the configuration has run, CMake will have generated Unix Makefiles for building Geant4 VMC. To run the build, simply execute make in the build directory:
```
$ make -jN
```
where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).

  - If you need more output to help resolve issues or simply for information, run make as
```
$ make -jN VERBOSE=1
```

5. Once the build has completed, you can install Geant4 VMC to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
```
$ make install
```

This will build geant4_vmc, g4root and mtroot packages.
For VMC examples see [VMC examples installation page](/installation/examples).

{{% children  %}}

<hr>

The instructions above apply to the installation since the version 3.0 built against Geant4 10.00.p03. For the installation of the previous versions see

- [Older Versions](geant4_vmc-old)
- [Special Installation](special-installations)

