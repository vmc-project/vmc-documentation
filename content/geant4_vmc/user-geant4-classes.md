+++
title = "User Geant4 Classes"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 8
+++

<p>The default Geant4 VMC behaviour, defined by the Geant4 user mandatory classes and user action classes implemented in Geant4 VMC, can be customized by a user by providing his own class derived from TG4RunConfiguration. <br />
<p>Such customization is recommended for including a user own physics list. User has also the possibility to override detector construction and/or primary generation action classes and use an existing Geant4 geometry/primary generator definition with VMC. In other cases, though the customisation is possible and allowed by the design, it has not been tested and so it is not recommended especially for the novice users. <a name="Impl_Regions"></a></p>

<h3> User Physics List </h3>
The example of including user own physics list is provided in the VMC example E03 in <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration2.html">  Ex03RunConfiguration2 </a> class..</p>
<p>In case, a user has registered his own physics list, he has a possibility to combine his own physics list with the TG4SpecialPhysicsList using TG4ComposedPhysics list as it is demonstarted in the VMC example E03, see <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration2.html">  Ex03RunConfiguration2 </a>, and he can then activate any special process as described above.( Another possibility, which requires more expertise, would be to register the special process physics constructor (eg. class TG4SpecialCutsPhysics) in a user own modular physics list.)</p>

<h3> User Detector Construction</h3>
<p> Including of user geometry construction is also demonstrated in the example E03 in <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration1.html">  Ex03RunConfiguration1 </a> class..</p>

<h3>Regions</h3>
<p>Since version 2.5, users now have a possibility  to define Geant4 regions by overriding the   <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4VUserRegionConstruction.html">  TG4VUserRegionConstruction </a> class. Definition and including of such a user region construction class in the VMC application is demonstrated in the example E03 in <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration3.html">  Ex03RunConfiguration3 </a> class.
