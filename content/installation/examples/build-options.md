+++
title = "Build Options"
menuTitle = ""
chapter = false
weight = 1
+++

Overview of the available options for building VMC examples and their default values:
```
VMC_WITH_Geant4       Build with Geant4            OFF
VMC_WITH_Geant3       Build with Geant3            OFF
VMC_WITH_Multi        Build with multiple engines  OFF
VMC_WITH_MTRoot       Build with MTRoot            ON
VMC_INSTALL_EXAMPLES  Install examples libraries and programs  ON
```

Geant4 VMC build options are automatically exported in `Geant4VMCConfig.cmake`. If Geant4 VMC  was built with VGM, the VGM installation path has to be provided via `-DVGM_DIR` option.
