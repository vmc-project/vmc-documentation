+++
title = "Installing Geant4 VMC"
menuTitle = "Geant4 VMC"
chapter = false
weight = 3
+++

Geant4 VMC requires the [VMC core package](/user-guide/vmc/vmc-library), [ROOT](https://root.cern.ch/) and [Geant4](http://geant4.web.cern.ch/) installed, and optionally, it can be built with [VGM](https://github.com/vmc-project/vgm). See below tips for configuration and installation of these packages.

In 2019, the [vmc core package](/user-guide/vmc/vmc-library) was separated from the ROOT source into a new stand-alone [vmc](https://github.com/vmc-project/vmc) package in the GitHub [vmc-project](https://github.com/vmc-project) organization. The motivation for this step was a gain in flexibility and faster workflow for new developments of multiple engine mode. Since ROOT version 6.18 the vmc package in ROOT is deprecated and it's compilation is optional. The VMC stand-alone is supported since Geant4 VMC 5.0.

Geant4 VMC uses [CMake](https://cmake.org/) to configure a build system for compiling and installing the headers, libraries and CMake configuration files. To install geant4_vmc:

1. First get the Geant4 VMC source from the  [Download page](/download/git-geant4_vmc). We will assume that the Geant4 VMC package sits in a subdirectory
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

  - Since ROOT 6.18, building the VMC library is optional. When using ROOT built with the vmc option enabled, then Geant4 VMC will automatically use the VMC library from ROOT, otherwise the VMC stand-alone library has to be provided using `-DVMC_DIR` cmake option.  

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

