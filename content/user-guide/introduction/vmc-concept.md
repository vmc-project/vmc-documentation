+++
title = "VMC Concept"
menuTitle = "VMC Concept"
chapter = false
weight = 1
+++

The Virtual Monte Carlo (VMC) allows to run different simulation Monte Carlo without changing the user code and therefore the input and output format as well as the geometry and detector response definition.

The core of the VMC is the category of classes **vmc**. It provides a set of interfaces which completely decouple the dependencies between the user code and the concrete Monte Carlo, two of which have the key role:

- [TVirtualMC](https://vmc-project.github.io/vmc/classTVirtualMC.html): Interface to the concrete Monte Carlo program
- [TVirtualMCApplication](https://vmc-project.github.io/vmc/classTVirtualMCApplication.html): Interface to the user's Monte Carlo application

The implementation of the TVirtualMC interface is provided for two Monte Carlo transport codes, GEANT3 and [Geant4](http://geant4.web.cern.ch/geant4/), with the VMC packages listed below. The implementation for the third Monte Carlo transport code, FLUKA, has been discontinued by the FLUKA team in 2010.

The other interfaces are implemented in the user application. The user VMC application is independent from concrete transport codes (GEANT3, Geant4, FLUKA). The transport code which will be used for simulation is selected at run time - when processing a ROOT macro where the concrete Monte Carlo is instantiated.

## VMC and TGeo

The VMC is fully integrated with the Root geometry package, [TGeo](https://root.cern/doc/master/group__Geometry__classes.html), and users can easily define their VMC application with TGeo geometry and this way of geometry definition is recommended for new users.

It is also possible to define geometry via Geant3-like functions defined in the VMC interface, however this way is kept only for backward compatibility and should not be used by new VMC users.

## Multiple VMCs

Since the development version the simulation can be shared among multiple different engines deriving from [TVirtualMC](https://vmc-project.github.io/vmc/classTVirtualMC.html) which are handled by a singleton [TMCManager](https://vmc-project.github.io/vmc/classTMCManager.html) object.

See more detailed description in [the dedicated section](/user-guide/vmc/multiple-vmc).
