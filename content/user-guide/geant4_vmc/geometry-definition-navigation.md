+++
title = "Geometry Definition & Navigation"
menuTitle = "Geometry Definition & Navigation"
chapter = false
hidden = false
showhidden = true
weight = 1
+++

### Geometry definition

The VMC supports two ways of geometry definition:

- via [Root geometry package (TGeo)](http://root.cern.ch/root/doc/chapter19.pdf)
- via [TVirtualMC interface](https://vmc-project.github.io/vmc/html/classTVirtualMC.html) (historically the first way)

The first (newer) way is recommended for new users, the way via VMC is kept for a backward compatibility.

Since the version 2.0, user can choose between Geant4 native navigation and G4Root navigation, if geometry is define via TGeo. The choice of the navigation is done via the option specified with creating <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4RunConfiguration.html">  TG4RunConfiguration </a> object (see  <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html">  examples </a> for more details):

- `geomVMCtoGeant4`   - geometry defined via VMC, G4 native navigation
- `geomVMCtoRoot`     - geometry defined via VMC, Root navigation
- `geomRoot`          - geometry defined via Root, Root navigation
- `geomRootToGeant4`  - geometry defined via Root, G4 native navigation
- `geomGeant4`        - geometry defined via Geant4, G4 native navigation

Below we shortly comment the implementation of these options:

- `geomVMCtoGeant4`:
  The interfaces to functions for geometry definitions provided by VMC were strongly inspired by Geant3; the implementation of these functions in Geant4 VMC is therefore made with use of the G3toG4 tool provided by Geant4.</li>

- `geomVMCtoRoot`:
  The implementation of this option uses [TGeoMCGeometry](https://root.cern.ch/doc/master/classTGeoMCGeometry.html) provided in the root/vmc package. The same class is used also by TGeant3TGeo and TFluka for supporting user geometry defined via VMC.

- `geomRoot`:
  If geometry is defined via Root and G4Root navigation is selected, Geant4 VMC only converts the parameters defined in [TGeoMedium](https://root.cern.ch/doc/master/classTGeoMedium.html) objects to Geant4 objects.

- `geomRootToGeant4`:
  The geometry defined via TGeo is converted in Geant4 geometry using the external <a href="http://ivana.home.cern.ch/ivana/VGM.html"> Virtual Geometry Model </a> (VGM), which has replaced the old one-way converters from Geant4 VMC (G4toXML, RootToG4), removed from Geant4 VMC with the version 1.7. In the VGM, these convertors has been generalized and improved.

- `geomGeant4`:
  User Geant4 detector construction class can be passed to Geant4 VMC via user defined run configuration class (see [User Geant4 Classes](/user-guide/geant4_vmc/user-geant4-classes)). If Geant4 VMC is built with VGM, geometry can be exported in Root using the built-in command:
  ```bash
  /vgm/generateRoot
  ```
  and reused in Geant3 or Fluka (when available) VMC simulation.

### Useful commands

Geant4 VMC implements various commands which allow users to get more information about their application setup:
```bash
/mcDet/printMaterials
/mcDet/printMaterialsProperties
/mcDet/printMedia
/mcDet/printVolumes
```
prints materials, material properties, tracking media, volumes

In Geant4 VMC, there is by default set a step limit 10 cm for all materials with density lower than 0.001 g/cm3. This default setting can be overridden via the following commands:
```bash
/mcDet/setLimitDensity 1.e-6 g/cm3
/mcDet/setMaxStepInLowDensityMaterials 1 mm
```

How to apply Geant4 commands in a Root user session is explained at the section on [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces).

### Geometry in XML

The VGM provides also the XML exporters which enable to generate XML files with geometry description in the AGDD or GDML formats. If Geant4 VMC is compiled with USE_VGM option, geometry can be exported to XML using the built-in commands:
```bash
/vgm/generateAGDD [volumeName]
/vgm/generateGDML [volumeName]
```

If volumeName is not specified, the whole geometry volume tree is exported.
The geometry can be then browsed and visualized using  <a href="http://hrivnac.home.cern.ch/hrivnac/Activities/Packages/GraXML">  GraXML tool </a>.

### "MANY" positions

As Geant4 does not support overlapping geometries, the positions with MANY option  are not allowed with Geant4 navigation.

In case of `geomVMCtoGeant4` option, user has a possibility to identify all ONLY volumes that overlap with the MANY ones using `TVirtualMC::Gsbool()` function for each overlap. This info is then used to perform automatically Boolean operations on the concerned solids. The volume with a "MANY" position can have only this position if Gsbool is used.

In case of `geomRootToGeant4` option, the MANY option is ignored and if present in geometry, Gean4 geometry will be incorrect.

There is no limitation on use of the MANY option with G4Root navigation (options `geomVMCtoRoot`, `geomRoot`).
