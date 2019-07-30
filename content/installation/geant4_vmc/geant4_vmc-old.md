+++
title = "Installing Geant4 VMC - Older Versions"
menuTitle = "Geant4 VMC Older Versions"
chapter = false
hidden = true
weight = 1
+++

<p>
The following instructions apply to the installation of versions < 3.0. For the required configurations of Root and Geant4 for these older versions see <a href="/drupal/content/special-installations"> Special installations </a> page.   

<ol>    
    <li> <h3>Getting source </h3>
    <p>
    First get the Geant4 VMC source from the 
    <a href="/drupal/content/download-vmc">Download page</a>.
    </p>
    <li> <h3>Root configuration </h3>
Since the version 2.9 there are no special configuration options for Root needed. 
<p>
The path to Root installation can be defined in two
complementary ways:<br>
a) With the environment variable ROOTSYS:
<bash>
ROOTSYS         ... path to Root
</bash>
b) With the Root configuration script
<bash>
root-config     ... Root configuration script
</bash> 
Note that if the ROOTSYS environment variable is defined the root-config script is not used.
<p>
For the older geant4_vmc versions (2.x - 2.8) see below.
</li>

<li> <h3>Geant4 configuration (with CMake) </h3>
To install Geant4 - download the source from <a href="http://geant4.web.cern.ch/geant4/support/download.shtml"> Geant4 Download page </a> and follow the            
<a href="http://geant4.web.cern.ch/geant4/UserDocumentation/UsersGuides/InstallationGuide/html/index.html"> Geant4 Installation Guide </a> together with the tips relevant to using it with VMC described below. <br />
<p>
Geant4 installation options: 
    <ul>
        <li><b>G3toG4 tool</b> <br />
        The G3toG4 package is used in geant4_vmc to support geometry definition via VMC with Geant4 native navigation
(geomVMCtoGeant4 option). It is required by default.
As the G3toG4 package is optional its build has to be activated when building Geant4 by the following CMake options:  
<bash>
-DGEANT4_USE_G3TOG4=ON
</bash>

<p>
Since the version 2.14, the user applications which build geometry via Root (or Geant4), can be built with Geant4 installed without G3toG4 package. The G3toG4 dependent code in geant4_vmc has to be then 
inactivated by setting the following environment variable:
<bash>
NO_G3TOG4   ... set to 1 to disable the code dependent on G3toG4
</bash>
If this variable is set the geomVMCtoGeant4 option is unavailable. 
</li><br>
     
    <li><b>OpenGL visualization </b> <br>
It is recommended to build Geant4 X11 OpenGL visualization driver used 
in the VMC examples. It is handled in Geant4 CMake build via the following CMake option:  
<bash>
-DGEANT4_USE_OPENGL_X11=ON
</bash>
</li><br>
    <li><b>Multi-threading </b> <br>
Geant4 VMC can be built against Geant4 installation in multi-threading mode (with -DG4MULTITHREADED cmake option). Dynamic loading of Geant4 libraries, as used in VMC, requires to change the Geant4 default option (defined in geant4/cmake/Modules/Geant4MakeRules_cxx.cmake)
<bash>
-ftls-model=initial-exec
</bash>with 
<bash>
-ftls-model=global-dynamic
</bash>
<p>
Geant4 VMC 2.15x version is not migrated to Geant4 multi-threading
and it can be built against both Geant4 sequential and Geant4 multi-threading installations. The Geant4 MT installation requires -ftls-model=global-dynamic model. The VMC application will run in a sequential mode in both cases.
</p>
<p>
Geant4 VMC 3.00.x version is migrated to Geant4 multi-threading
and can be built against both Geant4 sequential and Geant4 multi-threading
installations. The -ftls-model=global-dynamic model is required for
running VMC applications from Root session (in a "traditional" way 
with dynamic loading of libraries). Since 3.00.x version, CMake configuration files are provided to build VMC application main
program linked with all libraries, Geant4 built with -ftls-model=initial-exec model (default) can be then used.
</p>
Only VMC applications which are migrated to multi-threading mode
can be built and run against Geant4 multi-threading installation.
Non migrated application have to be built against Geant4 sequential
installation.
</ul>
<p>
The path to the Geant4 installation is defined with the Geant4 configuration script:
<bash>
geant4-config    ... Geant4 configuration script
</bash>
To make Geant4 binaries (geant4-config) and libraries available on your PATH and library path (LD_LIBRARY_PATH on Linux, DYLD_LIBRARY_PATH on Mac OS X), you should source the relevant script from your Geant4 installation (CMAKE_INSTALL_PREFIX):
<bash>
$ . bin/geant4.sh        ... on bourne shells (eg. bash)
or
$ source bin/geant4.csh  ... on C shells  
</bash>

<p> 
Note that <b>the G4INSTALL environment variable should NOT be set</b>
in this case as it would trigger building geant4_vmc against the Geant4 configuration via GNUmake build. 


<li> <h3> VGM (optional) </h3>
<p>
Since the version 2.0, you can choose to run Geant4 with the Geant4 native geometry navigation or the G4Root navigation.

<p> To run with the Geant4 native geometry navigation in case your geometry is defined via the Root geometry package, you will have to install the Virtual Geometry Model (VGM) package. See the <a href="http://ivana.home.cern.ch/ivana/VGM.html"> VGM Web site </a> how to do it.

<p>
VGM is used in Geant4 VMC for a geometry in memory conversion from Root TGeo objects to the Geant4 native geometry. More details about this can be found at the <a href="http://root.cern.ch/drupal/content/geometry-definition-navigation"> page on geometry definition and navigation</a>.

<p>
The following environment variables that defines the paths to used systems have to be set:
<bash>
VGM_INSTALL     ... path to VGM
USE_VGM         ... set to 1 to build VGM dependent code
</bash>
</li>

    </li>
    <li> <h3> Geant4 VMC </h3>
<p>
For the versions (2.9 - 2.15x), to install geant4_vmc:
<bash>
cd geant4_vmc
make
</bash>
This will build g4root, geant4_vmc and VMC examples.

<p>
For the older versions (2.x - 2.8) of geant4_vmc:
<bash>
cd geant4_vmc/source
make
</bash>
</li>
    <li> <h3> VMC Examples </h3>
<p>
The examples are provided within geant4_vmc package; to build all available examples: <br />
<bash>
cd geant4_vmc/examples
make
</bash> 
</ol>
