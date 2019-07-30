+++
title = "Installing Geant4 VMC - Special Installations"
menuTitle = "Geant4 VMC Special Installations"
chapter = false
hidden = true
weight = 2
+++

<ul>
<li> <h3>Root Configuration for geant4_vmc versions (2.x - 2.8) </h3>
The older geant4_vmc versions (2.x - 2.8) require Root to be configured with <b>g4root</b>  package; for this you have to specify the following configure options:<br />
<bash>
--enable-g4root \
--with-g4-incdir=$G4INSTALL/include \
--with-g4-libdir=$G4INSTALL/lib/Linux-g++ \
--with-clhep-incdir=$CLHEP_BASE_DIR/include \ 
</bash>           
    </li>

    <li> <h3>Geant4 configuration (version >= 9.5) <br>
             with manual installation (with GNUmake) </h3>
If you install Geant4 with the GNUmakefile build system and your own environment setting, you need to have set the following environment variables. This way of installation requires an experience in Geant4 and so it is not recommended for novice users.  
<bash>
G4LIB_BUILD_SHARED set to 1 (required)
G4LIB_USE_G3TOG4 set to 1 (required)
G4VIS_BUILD_OPENGLX_DRIVER set to 1 (recommended)
G4VIS_USE_OPENGLX set to 1 (recommended)
</bash> 

<p>
The path to Geant4 installation is then defined with the environment variables. 
<bash>
G4INSTALL        ... path to Geant4 installation
G4SYSTEM         ... Geant4 system flavor  
CLHEP_BASE_DIR   ... path to CLHEP
</bash>

If Geant4 is installed in a different path than the path to the source tree, the following additional variables have to be set: 
<bash>
G4LIB            ... path to Geant4 libraries (optional)
G4INCLUDE        ... path to Geant4 include files (option)
</bash>
</li>

<li> <h3>Geant4 configuration (version <= 9.4) </h3>
<p>
Installation of Geant4 + Geant4 VMC requires the <a href="http://cern.ch/clhep"> CLHEP </a> shared library.</li>

<p>
Note that Geant4 VMC requires some Geant4 installation options which are not set by default during the Geant4 installation procedure:
    <ul>
        <li>Geant4 shared libraries;<br />
        <li>All Geant4 header files in the include directory</li>
        <li>Build Geant4 with the use of the G3toG4 tool <br />
        <li>It is also recommended to build Geant4 X11 OpenGL visualization driver which is used in the VMC examples. 
    </ul>
<p>
    To get these options when using Configure program, you should reply <b>y</b> to the following questions. The Configure program will then generate automatically a script for an environment setup which should be sourced each time when using Geant4.  
<bash>
Do you want to install all Geant4 headers in one directory? [n] y (required)
Do you want to build shared libraries? [y]  y (required)
Enable building of visualization drivers? [y] y  (recommended)
Enable building of the X11 OpenGL visualization driver? [n] y (recommended)
Enable build of the g3tog4 utility module? [n]  y (required)
</bash>
<p>
    If you install Geant4 with your own environment setting, you need to have set the following environment variables. This way of installation requires an experience in Geant4 and so it is not recommended for novice users.  
<bash>
G4LIB_BUILD_SHARED set to 1 (required)
G4LIB_USE_G3TOG4 set to 1 (required)
G4VIS_BUILD_OPENGLX_DRIVER set to 1 (recommended)
G4VIS_USE_OPENGLX set to 1 (recommended)
</bash> 
</li>
