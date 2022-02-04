+++
title = "Build Options"
menuTitle = ""
chapter = false
weight = 1
+++

Geant4 VMC includes the G4Root package and examples, which are independent from Geant4 VMC and can be build and used stand-alone. Use of G4Root, VGM, Geant4 G3toG4, UI and VIS packages in Geant4 VMC library is optional and can be switched on/off during CMake build.

Overview of available options and their default values:
```
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
```
