+++
title = "Installing Geant4 VMC"
menuTitle = "Geant4 VMC"
chapter = false
weight = 3
+++

<p>
The following instructions apply to the installation since the version 3.0 built against Geant4 10.00.p03. For the installation of the previous versions see <a href="/drupal/content/installing-geant4vmc-old-versions"> Installing geant4_vmc - Old Versions</a>. 

<h2> Geant4 VMC </h2>

<p>
Geant4 VMC requires ROOT and Geant4 installed, and optionally, it can be built with VGM. See below tips for configuration and installation of these packages.
</p>
<p>
Since version 3.00, Geant4 VMC is installed with CMake. 
To install geant4_vmc:
<ol>
    <li> First get the Geant4 VMC source from the 
    <a href="/drupal/content/download-vmc">Download page</a>.
    We will assume that the Geant4 VMC package sits in a subdirectory
<bash>
/mypath/geant4_vmc
</bash>
    <li> Create build directory alongside our source directory
<bash>
$ cd /mypath
$ mkdir geant4_vmc_build
$ ls
 geant4_vmc geant4_vmc_build    
</bash>
    <li> To configure the build, change into the build directory and run CMake:
<bash>
$ cd /mypath/geant4_vmc_build 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/geant4_vmc_install /mypath/geant4_vmc
</bash>
<p> If ROOT and Geant4 environment was defined using thisroot.&#91;c&#93;sh and geant4.&#91;c&#93;sh scripts, there is no need to provide path to their installations. Otherwise, they can be provided using -DROOT_DIR and -DGeant4_DIR cmake options.
</p>
   <li> After the configuration has run, CMake will have generated Unix Makefiles for building Geant4 VMC. To run the build, simply execute make in the build directory:
<bash>
$ make -jN
</bash>
where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).
<p>
If you need more output to help resolve issues or simply for information, run make as
<bash>
$ make -jN VERBOSE=1
</bash>

<li> Once the build has completed, you can install Geant4 VMC to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
<bash>
$ make install
</bash>
</ol>

This will build geant4_vmc, g4root and mtroot packages.
For VMC examples see <a href="http://root.cern.ch/drupal/content/installing-and-running-examples">  VMC examples installation page </a>.

<h3> Geant4 VMC Build Options </h3>

<p>
Geant4 VMC includes G4Root and MTRoot packages, which are independent from Geant4 VMC and can be build and used stand-alone. Use of G4Root, VGM, Geant4 G3toG4, UI and VIS packages in Geant4 VMC library is optional and can be switched on/off during CMake build.

<p>
Overview of available options and their default values:
<bash>
Geant4VMC_BUILD_G4Root       Build G4Root        ON
Geant4VMC_BUILD_MTRoot       Build MTRoot        ON
Geant4VMC_BUILD_Geant4VMC    Build Geant4VMC     ON
Geant4VMC_BUILD_EXAMPLES     Build VMC examples  ON

Geant4VMC_USE_G4Root         Build with G4Root                ON
Geant4VMC_USE_VGM            Build with VGM                   OFF
Geant4VMC_USE_GEANT4_UI      Build with Geant4 UI drivers     ON
Geant4VMC_USE_GEANT4_VIS     Build with Geant4 Vis drivers    ON
Geant4VMC_USE_GEANT4_G3TOG4  Build with Geant4 G3toG4 library OFF

Geant4VMC_INSTALL_EXAMPLES   Install examples    ON
</bash>

<h2> Required and Optional packages </h2>
<p>
Geant4 VMC requires ROOT and Geant4 installed, and optionally, it can be built with VGM. See below tips for configuration and installation of these packages.
</p>
<p>
The path to required and optional packages installations can be defined in these complementary ways:<br>
a) Via path to the CMake configuration file
<bash>
Geant4_DIR      ... path to Geant4Config.cmake
ROOT_DIR        ... path to ROOTConfig.cmake
VGM_DIR         ... path to VGMConfig.cmake
</bash>

b) With their configuration script available in your PATH (Geant4 and Root):
<bash>
geant4-config   ... Geant4 configuration script
root-config     ... Root configuration script
</bash> 

c) With the environment variable ROOTSYS (Root only)
<bash>
ROOTSYS         ... path to Root
</bash>

To make the packages configuration scripts available in your PATH, you should source the relevant script from the packages installations (CMAKE_INSTALL_PREFIX or ROOTSYS):
<bash>
$ . bin/geant4.sh          ... Geant4 - on bourne shells (eg. bash)
$ . bin/thisroot.sh        ... Root    
or
$ source bin/geant4.csh    ... Geant4 - on C shells  
$ source bin/thisroot.csh  ... Root
</bash>

<h3>C++11 </h3>
<p>
Geant4 VMC, as well as ROOT and Geant4, have moved to the C++11 standard. 
The latest versions of all three packages use C++11 by default:
Geant4 VMC 3.3, Geant4 10.2 and ROOT 6.
 <p>
When mixing other versions of Geant4 and ROOT together, the same standard must 
be used for both packages. See below how the override the default setting 
when needed.

<h3>ROOT </h3>
To install ROOT - follow the            
<a href="http://root.cern.ch/building-root">
Root Installation Page </a>. 
    <ul>
        <li><b>C++11</b> <br />
Geant4 VMC 3.3 built against ROOT 5.x requires ROOT built with C++11 
(not default for this ROOT version), set via:
<bash>
-Dcxx11=ON
</bash>
option, when ROOT is built using CMake, or
<bash>
--enable-cxx11
</bash>
option, when ROOT is built using configure script.
</ul>
<h3>Geant4 </h3> 
To install Geant4 - download the source from <a href="http://geant4.web.cern.ch/geant4/support/download.shtml"> Geant4 Download page </a> and follow the            
<a href="http://geant4.web.cern.ch/geant4/UserDocumentation/UsersGuides/InstallationGuide/html/index.html"> Geant4 Installation Guide </a> together with the tips relevant to using it with VMC described below. <br />

    <ul>
        <li><b>C++11</b> <br />
Geant4 VMC 3.0 - 3.2 built against Geant4 10.0.x or 10.1.x and ROOT 6x requires
Geant4 built with C++11 (not default for these Geant4 versions), set via:
<bash>
-DGEANT4_BUILD_CXXSTD=c++11
</bash>
</li><br>

        <li><b>G3toG4 tool</b> <br />
        The G3toG4 package is used in geant4_vmc to support geometry definition via VMC with Geant4 native navigation. The build of G3toG4 package can be activated when building Geant4 with the following CMake options:  
<bash>
-DGEANT4_USE_G3TOG4=ON
</bash>
Since Geant4 VMC 3.0 the use of this package is optional.
</li><br>

    <li><b>OpenGL visualization </b> <br>
It is recommended to build Geant4 X11 OpenGL visualization driver used in the VMC examples. It is handled in Geant4 CMake build via the CMake option:  
<bash>
-DGEANT4_USE_OPENGL_X11=ON
</bash>
</li><br>
    <li><b>Multi-threading </b> <br>
Geant4 VMC can be built against Geant4 installation in multi-threading mode which is handled in Geant4 CMake build via the CMake option: 
<bash>
-DG4MULTITHREADED=ON
</bash>

Dynamic loading of Geant4 libraries, as used in VMC, requires to change the Geant4 default TLS model, initial-exec, with global-dynamic via the CMake option: 
<bash>
-DGEANT4_BUILD_TLS_MODEL=global-dynamic
</bash>

<p>
Geant4 VMC 3.00.x version is migrated to Geant4 multi-threading
and can be built against both Geant4 sequential and Geant4 multi-threading installations. The global-dynamic model is required for
running VMC applications from Root session (in a "traditional" way 
with dynamic loading of libraries). When the VMC application main
program is linked with all libraries, Geant4 built with the default TLS model can be used.
</p>

<p>
The VMC application will run automatically in MT mode when Geant4 VMC is built against Geant4 MT. See <a href="https://root.cern.ch/multi-threaded-processing"> the page on Multi-threaded processing </a> how this default behaviour can be changed in the configuration of the application. 
</li>
</ul>

<h3> VGM (optional) </h3>

<p> VGM is used in Geant4 VMC for a geometry in memory conversion from Root TGeo objects to the Geant4 native geometry. This conversion is performed when users geometry is defined via the Root geometry package
and  Geant4 native geometry navigation is selected. 

<p> To install VGM - follow the installation instructions on  <a href="http://ivana.home.cern.ch/ivana/VGM.html"> the VGM Web site </a>.
To build Geant4 VMC with VGM, you have to select the option:
<bash>
-DGeant4VMC_USE_VGM=ON
</bash>

For the old instructions see:

- [Older Versions](geant4_vmc-old)
- [Special Installation](special-installations)

{{% children  %}}
