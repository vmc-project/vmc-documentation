+++
title = "Required and Optional Packages"
menuTitle = ""
chapter = false
weight = 2
+++

Geant4 VMC requires [ROOT](https://root.cern.ch/) and [Geant4](http://geant4.web.cern.ch/) installed, and optionally, it can be built with [VGM](https://github.com/vmc-project/vgm). See below tips for configuration and installation of these packages.

The path to required and optional packages installations can be defined in these complementary ways:

a) Via path to the CMake configuration file
```
Geant4_DIR      ... path to Geant4Config.cmake
ROOT_DIR        ... path to ROOTConfig.cmake
VGM_DIR         ... path to VGMConfig.cmake
```

b) With their configuration script available in your PATH (Geant4 and Root):
```
geant4-config   ... Geant4 configuration script
root-config     ... Root configuration script
```

c) With the environment variable ROOTSYS (Root only)
```
ROOTSYS         ... path to Root
```

To make the packages configuration scripts available in your PATH, you should source the relevant script from the packages installations (CMAKE_INSTALL_PREFIX or ROOTSYS):
```
$ . bin/geant4.sh          ... Geant4 - on bourne shells (eg. bash)
$ . bin/thisroot.sh        ... Root    
or
$ source bin/geant4.csh    ... Geant4 - on C shells  
$ source bin/thisroot.csh  ... Root
```

### C++11

Geant4 VMC, as well as ROOT and Geant4, have moved to the C++11 standard. 
The latest versions of all three packages use C++11 by default:
Geant4 VMC 3.3, Geant4 10.2 and ROOT 6.

When mixing other versions of Geant4 and ROOT together, the same standard must 
be used for both packages. See below how the override the default setting 
when needed.

### ROOT
To install ROOT - follow the [Root Installation Page](http://root.cern.ch/building-root)

  - **C++11** <br />
    Geant4 VMC 3.3 built against ROOT 5.x requires ROOT built with C++11 (not default for this ROOT version), set via:
```
-Dcxx11=ON
```
option, when ROOT is built using CMake, or
```
--enable-cxx11
```
option, when ROOT is built using configure script.

### Geant4
To install Geant4 - download the source from [Geant4 Download page](http://geant4.web.cern.ch/geant4/support/download.shtml) and follow the [Geant4 Installation Guide](http://geant4.web.cern.ch/geant4/UserDocumentation/UsersGuides/InstallationGuide/html/index.html) together with the tips relevant to using it with VMC described below.

  - **C++11** <br />
Geant4 VMC 3.0 - 3.2 built against Geant4 10.0.x or 10.1.x and ROOT 6x requires
Geant4 built with C++11 (not default for these Geant4 versions), set via:
```
-DGEANT4_BUILD_CXXSTD=c++11
```

  - **G3toG4 tool**<br />
The G3toG4 package is used in geant4_vmc to support geometry definition via VMC with Geant4 native navigation. The build of G3toG4 package can be activated when building Geant4 with the following CMake options:  
```
-DGEANT4_USE_G3TOG4=ON
```
Since Geant4 VMC 3.0 the use of this package is optional.

  - **OpenGL visualization** <br>
It is recommended to build Geant4 X11 OpenGL visualization driver used in the VMC examples. It is handled in Geant4 CMake build via the CMake option:  
```
-DGEANT4_USE_OPENGL_X11=ON
```

  - **Multi-threading** <br>
Geant4 VMC can be built against Geant4 installation in multi-threading mode which is handled in Geant4 CMake build via the CMake option: 
```
-DG4MULTITHREADED=ON
```

    Dynamic loading of Geant4 libraries, as used in VMC, requires to change the Geant4 default TLS model, initial-exec, with global-dynamic via the CMake option: 
```
-DGEANT4_BUILD_TLS_MODEL=global-dynamic
```

    Geant4 VMC 3.00.x version is migrated to Geant4 multi-threading and can be built against both Geant4 sequential and Geant4 multi-threading installations. The global-dynamic model is required for running VMC applications from Root session (in a "traditional" way  with dynamic loading of libraries). When the VMC application main program is linked with all libraries, Geant4 built with the default TLS model can be used.

    The VMC application will run automatically in MT mode when Geant4 VMC is built against Geant4 MT. See <a href="https://root.cern.ch/multi-threaded-processing"> the page on Multi-threaded processing </a> how this default behaviour can be changed in the configuration of the application. 

### VGM (optional)

VGM is used in Geant4 VMC for a geometry in memory conversion from Root TGeo objects to the Geant4 native geometry. This conversion is performed when users geometry is defined via the Root geometry package and Geant4 native geometry navigation is selected. 

To install VGM - follow the installation instructions on the [VGM Web site](https://github.com/vmc-project/vgm).

To build Geant4 VMC with VGM, you have to select the option:
```
-DGeant4VMC_USE_VGM=ON
```
