+++
title = "Verbosity for Developers"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 14
+++

For many Geant4 VMC classes (like for Geant4 classes) the user can select a higher verbosity level and activate various printings which can help in understanding or debugging his application. Here we list the commands useful rather for Geant4 VMC developers, for the commands useful for users see [Verbosity](/user-guide/geant4_vmc/verbosity).

- ```
  /mcVerbose/SDConstruction  2
  ```
  - Print the volumes IDs (implemented via G4VSensitiveDetector objects) and the maps between volumes and volume IDs.

- ```
  /mcVerbose/runManager 3
  ```
  - Print the logical volume store.

- ```
  /mcVerbose/trackManager 2
  ```
  - Print particleID info from `SetTrackInformation`.

- ```
  /mcVerbose/physicsManager 2
  ```
  - Print the parameter name and value from calls to Gstpar. 

- ```
  /mcVerbose/particlesManager 2
  ```
  - Print the map between the names of special particles in Root and Geant4 (like geantino, opticalphoton, etc.); print also info about particles and ions defined by user.
    
- ```
  /mcVerbose/physicsProcessControlMap 1
  /mcVerbose/physicsProcessMCMap 1
  /mcVerbose/physicsStepLimiter 1
  /mcVerbose/physicsUserParticles 1
  ```
  - Print info about each constructed special process; in case of map processes (physicsProcessControlMap, physicsProcessMCMap) print also whether the mapping was successful (all physics processes in the user physics list were identified in the maps).

