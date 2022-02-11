+++
title = "VMC Project"
menuTitle = "VMC Project"
chapter = false
weight = 1
+++

The [vmc-project](https://github.com/vmc-project) GitGub organization provides the following packages:

## vmc [->](https://github.com/vmc-project/vmc)

The core of the VMC provides the **interfaces** and classes independent from the Monte Carlo transport code.

See more in the [VMC Core](../../vmc-core) section.

## geant3 [->](https://github.com/vmc-project/geant3)

The updated version of **Geant3.21** that includes several bug fixes compared to the standard version in CERNLIB. The directory TGeant3 contains the classes which **implement the TVirtualMC interface** for Geant3

See more in the [Geant3](../../geant3+vmc) section.


## geant4_vmc [->](https://github.com/vmc-project/geant4_vmc)

Geant4 VMC contains the classes which **implement the TVirtualMC interface for [Geant4](http://geant4.cern.ch)**.

For historical reasons, it also includes the **G4Root** package, which is independent from Geant4 VMC and can be built and used stand-alone, and **VMC Examples**, that can also be built independently and do not require Geant4 installation in case you want to run them with GEANT3.

See more in the [Geant4 VMC](../../geant4_vmc), [G4Root](../../g4root) and [Examples](../examples) sections.

## Other packages

In addition to these VMC packages, the vmc-project includes also the **geometry conversion tool [VGM](https://github.com/vmc-project/vgm)** and the **VMC and VGM documentation** repositories.
