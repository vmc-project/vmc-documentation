+++
title = "Geant3 + VMC Content"
menuTitle = ""
chapter = false
weight = 6
#pre = "<b>6. </b>"
+++

{{% notice warning %}}
Only the **Geant3.21** code itself and the implementation of the [TVirtualMC](https://root.cern/doc/master/classTVirtualMC.html) interface, **TGeant3**, provided in [geant3](https://github.com/vmc-project/geant3) are maintained by the VMC project.
{{% /notice %}}

### Geant3.21

The updated version of Geant3.21 that includes several bug fixes compared to the standard version in CERNLIB.
In this version all Geant3 gxxxxx routines have been renamed g3xxxxx.

The old `Makefile` system was replaced with a `CMake` based system since version 2.0.

See also [Geant3.21 User Guide](/geant.pdf) 

### TGeant3 (Geant3 VMC)

The directory `TGeant3` contains the classes TGeant3 and TGeant3TGeo,
which implement the  TVirtualMC interface, see more about VMC at: <br/>
[https://root.cern.ch/vmc](https://root.cern.ch/vmc)

### Examples

The directory `examples` includes a set of FORTRAN examples. These examples are not maintained and tested in the VMC project test suites. Instead, the geant3 package is tested using the test suites defined in [geant4_vmc/examples](/examples).

