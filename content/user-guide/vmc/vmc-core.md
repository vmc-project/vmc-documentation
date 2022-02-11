+++
title = "VMC Core"
menuTitle = ""
chapter = false
weight = 2
#pre = "<b>3.1 </b>"
+++

The core of the VMC, `vmc`, provides a set of interfaces which completely decouple the dependencies between the user code and the concrete Monte Carlo:

- [TVirtualMC](https://vmc-project.github.io/vmc/classTVirtualMC.html): Interface to the concrete Monte Carlo program
- [TVirtualMCApplication](https://vmc-project.github.io/vmc/classTVirtualMCApplication.html): Interface to the user's Monte Carlo application
- [TVirtualMCStack](https://vmc-project.github.io/vmc/classTVirtualMCStack.html): Interface to the particle stack
- [TVirtualMCDecayer](https://vmc-project.github.io/vmc/classTVirtualMCDecayer.html): Interface to the external decayer
- [TVirtualMCSensitiveDetector](https://vmc-project.github.io/vmc/classTVirtualMCSensitiveDetector.html): Interface to the user's sensitive detector

The implementation of the TVirtualMC interface is provided for two Monte Carlo transport codes, GEANT3 and [Geant4](http://geant4.web.cern.ch/geant4/), with the VMC packages listed below. The implementation for the third Monte Carlo transport code, FLUKA, has been discontinued by the FLUKA team in 2010.

The other three interfaces are implemented in the user application. The user has to implement two mandatory classes: the **MC application** (derived from TVirtualMCApplication) and the **MC stack** (derived from TVirtualMCStack), optionally an external decayer (derived from TVirtualMCDecayer) can be introduced. The user VMC application is independent from concrete transport codes (GEANT3, Geant4, FLUKA). The transport code which will be used for simulation is selected at run time - when processing a ROOT macro where the concrete Monte Carlo is instantiated.

The relationships between the interfaces and their implementations are illustrated in the class diagrams: User MC application , Virtual MC , demonstarting the decoupling between the user code and the concrete transport code.

## Multiple VMCs

Since the development version the simulation can be shared among multiple different engines deriving from [TVirtualMC](https://vmc-project.github.io/vmc/classTVirtualMC.html) which are handled by a singleton [TMCManager](https://vmc-project.github.io/vmc/classTMCManager.html) object.

See more detailed description in [the dedicated section](/user-guide/vmc/multiple-vmc).

## Root IO Manager

Root IO manager class, [TMCRootManager](https://vmc-project.github.io/vmc/classTMCRootManager.html), facilitates use of ROOT IO in VMC examples and it also handles necessary locking in multi-threaded applications.

It was first provided in Geant4 VMC in the MTRoot sub-package and moved to the VMC core (since vmc 2.0 and geant4_vmc 6.0). 

