+++
title = "User Geant4 Classes"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 8
+++

The default Geant4 VMC behaviour, defined by the Geant4 user mandatory classes and user action classes implemented in Geant4 VMC, can be customized by a user by providing his own class derived from `TG4RunConfiguration`.

Such customization is recommended for including a user own physics list. User has also the possibility to override detector construction and/or primary generation action classes and use an existing Geant4 geometry/primary generator definition with VMC. In other cases, though the customisation is possible and allowed by the design, it has not been tested and so it is not recommended especially for the novice users.

### User Physics List

The example of including user own physics list is provided in the VMC example E03 in [Ex03RunConfiguration2](http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration2.html) class.

In case, a user has registered his own physics list, he has a possibility to combine his own physics list with the `TG4SpecialPhysicsList` using `TG4ComposedPhysics` list as it is demonstrated in the VMC example E03, see [Ex03RunConfiguration2](http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration2.html), and he can then activate any special process as described above.( Another possibility, which requires more expertise, would be to register the special process physics constructor (eg. class `TG4SpecialCutsPhysics`) in a user own modular physics list.)

### User Detector Construction

Including of user geometry construction is also demonstrated in the example E03 in [Ex03RunConfiguration1](http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration1.html) class.

### Regions

Since version 2.5, users have a possibility to define Geant4 regions by overriding the  [TG4VUserRegionConstruction](http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4VUserRegionConstruction.html) class. Definition and including of such a user region construction class in the VMC application is demonstrated in the example E03 in [Ex03RunConfiguration3](http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration3.html) class.

