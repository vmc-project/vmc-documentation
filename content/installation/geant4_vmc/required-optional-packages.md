+++
title = "Required and Optional Packages"
menuTitle = ""
chapter = false
weight = 2
+++

Geant4 VMC requires [ROOT](https://root.cern.ch/) and [Geant4](http://geant4.web.cern.ch/) installed, and the standalone [VMC core package](/user-guide/vmc/vmc-core), and optionally, it can be built with [VGM](https://github.com/vmc-project/vgm). See below tips for configuration and installation of these packages.

The path to required and optional packages installations can be defined in the standard CMake way, via the paths to the CMake configuration file:
```
Geant4_DIR      ... path to Geant4Config.cmake
ROOT_DIR        ... path to ROOTConfig.cmake
VMC_DIR         ... path to VMCConfig.cmake
VGM_DIR         ... path to VGMConfig.cmake
```

CMake will also find the CMake configuration files (for ROOT and Geant4) if you source the relevant script from the packages installations (CMAKE_INSTALL_PREFIX) so that they are available in the sestem paths:
```
$ . bin/geant4.sh          ... Geant4 - on bourne shells (eg. bash)
$ . bin/thisroot.sh        ... Root    
or
$ source bin/geant4.csh    ... Geant4 - on C shells  
$ source bin/thisroot.csh  ... Root
```

### Supported versions and C++ standard

Geant4 VMC 6.8 was tested with VMC 2.1, Geant4 11.4, ROOT 6.36/06 and VGM 5.4. These are the release's tested versions, not minimum requirements for every dependency.

Use CMake 3.16 or newer and a compiler supporting C++17. Geant4 11.4 defaults to C++17; use the same C++ standard for ROOT, Geant4, VMC and your application. See the [Geant4 build instructions](https://geant4.web.cern.ch/documentation/dev/ig_html/InstallationGuide/installguide.html) for supported newer standards and build options.

The C++11 instructions for older releases are retained under [Older Versions](/installation/geant4_vmc/geant4_vmc-old#historical-c11-configuration).

### ROOT and standalone VMC

Follow the [ROOT installation instructions](https://root.cern/install/) and then [install standalone VMC](/installation/vmc). Current Geant4 VMC requires standalone VMC and rejects a ROOT installation with the old embedded VMC enabled. Set `VMC_DIR` to the directory containing `VMCConfig.cmake` if CMake does not find it automatically.

### Geant4

Follow the [Geant4 installation guide](https://geant4.web.cern.ch/documentation/dev/ig_html/InstallationGuide/installguide.html), together with the VMC-specific options below.

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
-DGEANT4_BUILD_MULTITHREADED=ON
```

Dynamic loading of Geant4 libraries, as used in VMC, requires to change the Geant4 default TLS model, initial-exec, with global-dynamic via the CMake option: 
```
-DGEANT4_BUILD_TLS_MODEL=global-dynamic
```

Geant4 VMC 3.00.x version is migrated to Geant4 multi-threading and can be built against both Geant4 sequential and Geant4 multi-threading installations. The global-dynamic model is required for running VMC applications from Root session (in a "traditional" way  with dynamic loading of libraries). When the VMC application main program is linked with all libraries, Geant4 built with the default TLS model can be used.

The VMC application will run automatically in MT mode when Geant4 VMC is built against Geant4 MT. See [the page on Multi-threaded processing](/user-guide/geant4_vmc/multi-threaded-processing/) how this default behaviour can be changed in the configuration of the application. 

### VGM (optional)

VGM is used in Geant4 VMC for a geometry in memory conversion from Root TGeo objects to the Geant4 native geometry. This conversion is performed when users geometry is defined via the Root geometry package and Geant4 native geometry navigation is selected. 

To install VGM - follow the installation instructions on the [VGM Web site](https://github.com/vmc-project/vgm).

To build Geant4 VMC with VGM, you have to select the option:
```
-DGeant4VMC_USE_VGM=ON
```
