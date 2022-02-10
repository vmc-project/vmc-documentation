+++
title = "Magnetic Field"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 1
+++

The user magnetic field is in VMC defined via [TVirtualMagField](http://root.cern.ch/root/html/TVirtualMagField.html) interface and then set to VMC using [TVirtualMC::SetMagField(TVirtualMagField\*)](https://vmc-project.github.io/vmc/classTVirtualMC.html) function. The propagation of tracks inside the magnetic field in Geant4 can be performed to a user-defined accuracy. See more details in the [Electromagnetic Field section](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/html/Detector/electroMagneticField.html) in the Geant4 User Guide for Application Developers. Note that while Geant4 allows magnetic, electric and electromagnetic fields, the VMC is currently limited to magnetic fields only.

### Local fields

Since Geant4 VMC version 3.2, it is possible to define local magnetic fields. The local field is defined in the same way as a global field, via [TVirtualMagField](http://root.cern.ch/root/html/TVirtualMagField.html) interface, but then it has to be associated with a selected volume using [TGeoVolume::SetField(TObject\*)](https://root.cern.ch/root/htmldoc/TGeoVolume.html) function. The local magnetic field is applied also to all volume daughters, it is possible to combine a global field with one or more local fields. Note that the local fields are supported only with Geant4 and an equivalent global magnetic field has to be provided for Geant3 simulation.

The local fields defined in Root geometry are not taken into account automatically (to avoid unnecessary processing of the volume tree in applications which do not define local magnetic fields) and their usage has to be activated via:
```
/mcDet/setIsLocalMagField true 
```

Since Geant4 VMC version 3.5, it is possible to propagate the zero magnetic field defined in tracking media in Geant4 geometry. A local zero magnetic field is set to all logical volumes associated with the tracking medium with 'ifield = 0' if a global magnetic field is defined. This feature (not switched on by default) can be activated via:
```
/mcDet/setIsZeroMagField true 
```

### Customization of Field Parameters

In Geant4 VMC, user can customize the default integration method and the default accuracy parameters with a set of dedicated commands. It is a user responsibility to choose the type of equation of motion and the integration method compatible with the user field. (The defined commands are not limited to magnetic field only in order to make easier an eventual future extension of VMC to electric and electromagnetic fields.)

The available commands:

- Select the integrator (stepper) of particle's equation of motion; the default stepper is G4ClassicalRK4:
  ```
  /mcMagField/stepperType stepperType
  ```

- Select the type of equation of motion of a particle in a field; the default is G4Mag_UsualEqRhs:
  ```
  /mcMagField/equationType eqType
  ```

- Set the accuracy parameters delta intersection and delta one step of the G4 Field Manager. The default values in Geant4 are 0.001*mm and 0.01*mm respectively.
  ```
  /mcMagField/setDeltaIntersection value [unit]
  /mcMagField/setDeltaOneStep value [unit]
  ```

- Set the step minimum and delta chord parameters to G4 Chord Finder. The default values in Geant4 are 0.01*mm and 0.25*mm respectively.:
  ```
  /mcMagField/setStepMinimum value [unit]
  /mcMagField/setDeltaChord  value [unit]
  ```

- Set the MinimumEpsilonStep and the MaximumEpsilonStep parameters of the G4 Field Manager. The default values in Geant4 are 5.0e-5 and 0.001 respectively.
  ```
  /mcMagField/setMinimumEpsilonStep value
  /mcMagField/setMaximumEpsilonStep value
  ```

- Activate utilizing of a "cached magnetic field" (available since version 3.2) . The cached field value (the value from a previous call) is returned in case a new call is performed for a point which distance from a previous one is smaller than the value of the "ConstDistance" parameter.
  ```
  /mcMagField/setConstDistance value [unit]
  ```

To customize local field parameters, a command directory for the local field has to be created first using:
```
/mcDet/createMagFieldParameters volumeName
```

The same commands as for a global field are then available under this directory:
```
/mcMagField/volumeName/stepperType stepperType
/mcMagField/volumeName/equationType eqType
...
```

Since Geant4 VMC version 3.2, users can also provide their own magnetic field equation of motion and/or its integrator. These objects should be instantiated in a detector construction class derived from [TG4DetConstruction](https://vmc-project.github.io/geant4_vmc/g4vmc_html/classTG4DetConstruction.html). This use case is demonstrated in the E03 example in [Ex03G4DetectorConstruction](https://vmc-project.github.io/geant4_vmc/examples_html/classEx03G4DetectorConstruction.html) and [Ex03RunConfiguration4](https://vmc-project.github.io/geant4_vmc/examples_html/classEx03RunConfiguration4.html) classes. How to include user Geant4 classes in a VMC application is explained at the section on  [User Geant4 classes](/user-guide/geant4_vmc/user-geant4-classes).


### Control of the parameters for killing looping particles

Since Geant4 10.5, users can change the thresholds for killing ‘looping’ tracks, which criteria can be used to ensure that a track continues to propagate and for how many steps an ‘important’ track that is ‘looping’ can survive (see more details in [Transportation in Magnetic Field - Further Details section](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/html/Appendix/magneticTransportation.html#fine-grained-control-of-the-parameters-for-killing-looping-particles) in Geant4 Guide for Application Developers).

The following UI commands were introduced in Geant4 VMC development version, and back-ported to Geant4 VMC 5.0.p5, for this control.

The commands for using preset thresholds for killing loopers:

```
/mcPhysics/useLowLooperThresholds
/mcPhysics/useHighLooperThresholds
```

The commands for modifying the parameters for killing looping particles

```
/mcRun/setLooperThresholdWarningEnergy value unit
/mcRun/setLooperThresholImportantEnergy value unit
/mcRun/setNumberOfLooperThresholdTrials value
```

These commands must be called before the run initialization, so that Geant4 VMC performs the setting at the right time.

----

How to apply Geant4 commands in a Root user session is explained at the section on [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces).

