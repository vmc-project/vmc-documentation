+++
title = "Special Cuts and Regions"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 7
+++

The way of applying cuts is different in Geant3 and Geant4. In Geant3, the cuts are defined as a limit in energy, which is applied both as an energy threshold (a secondary particle is not produced if its energy is beyond the threshold) and a tracking cut (a particle is stopped when its energy gets below the cut).

Inn Geant4, there is defined a unique cut in range which is then converted to an energy threshold per particle and material. The advantage is that you keep the same spatial resolution of your energy deposit over the whole detector. It is also possible to define cuts per regions, as in big experimental setups you may want to speed up your simulation by setting a higher cut in the support structures etc. See more details at the Geant4 User Guide for Application Developers, [section Production Threshold versus Tracking Cut](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/html/TrackingAndPhysics/thresholdVScut.html).

### VMC cuts

The VMC provides a possibility for a user to define cuts in Geant3 way. The cuts can be defined globally or per tracking medium via the following [TVirtualMC](https://vmc-project.github.io/vmc/classTVirtualMC.html) functions:
{{< highlight cpp >}}
gMC->SetCut(cutname, cutValue);
gMC->Gstpar(medId, cutName, cutValue);
{{< /highlight >}}

The user defined VMC cuts and their interpretation in Geant4 can be viewed with the following commands: 

To print the cut values for the given cut type (cutName) and control values for the given control type (controlName) for all tracking media:
```bash
/mcDet/printCuts  cutName
/mcDet/printControls  controlName
```

To print global cuts and global process controls:
```bash
/mcPhysics/printGlobalCuts
/mcPhysics/printGlobalControls
```

To print the user limits (including the VMC cuts and controls) set for the specified volume:
```bash
/mcPhysics/printVolumeLimits volName
```

### Geant4 cuts

**By default, Geant4 VMC ignores the VMC cuts** and applies only the global cut in range defined in the physics list. The default global cut value in Geant4 VMC is 1\*mm for all particles which cut is applied for (gamma, e-, e+, proton). User can change this default value for each particle separately or set a new value for all using the commands:
```bash
/mcPhysics/rangeCutForGamma    value    
/mcPhysics/rangeCutForElectron value
/mcPhysics/rangeCutForPositron value
/mcPhysics/rangeCuts value
```

### Applying VMC cuts in Geant4

In order to take the VMC cuts into account, the user has to **activate the special cuts process** by switching it on in his `g4Config.C` (see the section on [Physics list selection](/user-guide/geant4_vmc/physics_lists). This special cuts process applies the VMC cuts as tracking cuts using `G4UserLimits`.

Since Geant4 VMC version 6.5, based on Geant4 version 11.2, when the special cuts process is activated, the VMC cuts are also directly set as production cuts to `G4ProductionCutsTable.`

First, Geant4 VMC defines regions per materials. Each region contains all volumes that have the same material. In the next step, these regions are used to create materials cuts couples in `G4ProductionCutsTable` and then, in the third step, the energy cut vector is constructed (by filling the VMC cuts in the order of already constructed materials cuts couples vector) and then set to  `G4ProductionCutsTable` using its `SetEnergyCutVector()` function.
This is performed in [TG4RegionsManager2](https://vmc-project.github.io/geant4_vmc/g4vmc_html/classTG4RegionsManager2.html) (new in Geant4 VMC version 6.5).

User can select several levels of verbosity to control the process of regions definition:
```bash
/mcVerbose/regionsManager level
  level = 0  no output
          1  number of regions added via VMC
          2  the list of all volumes and their associated regions
```

Since Geant4 VMC version 6.3, users can print or save the regions data in a file. The default filename, `regions.dat` can be also changes with a command:
```
/mcRegions/print [true|false]
/mcRegions/save [true|false]
/mcRegions/setFileName fileName
```

The consistency of the volume material and associated regions can be also checked with the `check` command.
```
/mcRegions/check [true|false]
```

The old method of applying cuts via ranges can be activated in `g4Config.C` using `TG4RunConfiguration::SetSpecialCutsOld()`.

### Applying VMC cuts in Geant4 via ranges (Old method)

With older versions, the cut energy is first converted in range and then these range production cuts are set to the region. This conversion is performed in [TG4RegionsManager](https://vmc-project.github.io/geant4_vmc/g4vmc_html/classTG4RegionsManager.html) using the Geant4 converter classes `G4RToEConvForElectron` and `G4RToEConvForGamma` by iterating within a given range interval up to a given precision. User can change the default precision (=
the number of iterations), which is set to 5 (was 2 up the version 6.2) with the command:
```bash
/mcRegions/setRangePrecision value
```

The conversion of VMC cuts to the regions follows these rules:

- If the VMC energy cut defined by user results in a range cut smaller than the default range cut value defined in user physics list, the VMC cut is ignored and the default range cut is used.

- The range cut is first evaluated within the range 1e-03mm to 1m; when the range cut order is found, it is refined up to given precision (5 orders of magnitude by default) within 10 values of each order and the range value with the closest energy still smaller than the VMC cut is chosen.<br>
It may happen that a value cannot be refined up to the given precision, then the best found value is returned.

- The regions are defined only in case when the VMC cuts result in range cuts different from the range cuts in the default region; then the region includes all logical volumes with a given material.

- As the range cuts do not match precisely to user defined energy cuts, the specialCuts process applies the energy cuts as tracking cuts when a particle with energy cut below threshold is generated.

User can select several levels of verbosity to control the process of regions definition:
```bash
/mcVerbose/regionsManager level   
  level = 0  no output
          1  number of regions added via VMC
          2  the list of all volumes, the cuts in energy and
             calculated cuts in range
          3  all evaluated energy values
```

By default, the computed range cut for e- is applied also to e+ and proton. This feature can be switched off by the UI commands:
```bash
/mcRegions/applyForPositron false
/mcRegions/applyForProton false
````

In addition to the commands presented in previous sections, users can also load the regions data from the a file.
```
/mcRegions/load [true|false]
```

The `check` commands permorms also a check for the match between the cuts computed from ranges and the input VMC cuts. When this check is activated all cut values that differ with more than a given tolerance will be printed in the output. The default value of this tolerance (1%) can be also tuned:
```
/mcRegions/check [true|false]
/mcRegions/setEnergyTolerance value
```

It is also possible to dump the regions properties for a specified volume or to activate printing properties of all regions:
```bash
/mcRegions/dump volumeName
/mcRegions/print true|false
```

How to apply Geant4 commands in a Root user session is explained at the section on [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces).
