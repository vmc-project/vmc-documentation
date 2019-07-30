+++
title = "Magnetic Field"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 1
+++

<p>The user magnetic field is in VMC defined via <a href="http://root.cern.ch/root/html/TVirtualMagField.html"> TVirtualMagField </a> interface and then set to VMC using <a href="http://root.cern.ch/root/html/TVirtualMC.html">TVirtualMC::SetMagField(TVirtualMagField*)</a> function. The propagation of tracks inside the magnetic field in Geant4 can be performed to a user-defined accuracy. See more details at the Geant4 User Guide for Application Developers, <a href="http://geant4.web.cern.ch/geant4/UserDocumentation/UsersGuides/ForApplicationDeveloper/html/ch04s03.html"> section 4.3. Electromagnetic Field </a>. Note that while Geant4 allows magnetic, electric and electromagnetic fields, the VMC is currently limited to magnetic fields only.</p>

<p>Since Geant4 VMC version 3.2, it is possible to define local magnetic fields. The local field is defined in the same way as a global field, via <a href="http://root.cern.ch/root/html/TVirtualMagField.html"> TVirtualMagField </a> interface, but then it has to be associated with a selected volume using <a href="https://root.cern.ch/root/htmldoc/TGeoVolume.html"> TGeoVolume::SetField(TObject*)</a> function. The local magnetic field is applied also to all volume daughters, it is possible to combine a global field with one or more local fields. Note that the local fields are supported only with Geant4 and an equivalent global magnetic field has to be provided for Geant3 simulation.</p>

<p>The local fields defined in Root geometry are not taken into account automatically (to avoid unnecessary processing of the volume tree in applications which do not define local magnetic fields) and their usage has to be activated via: 
<bash>/mcDet/setIsLocalMagField true 
</bash></p>

<p>Since Geant4 VMC development version, it is possible to propagate the zero magnetic field defined in tracking media in Geant4 geometry. A local zero magnetic field is set to all logical volumes associated with the tracking medium with 'ifield = 0' if a global magnetic field is defined. This feature (not switched on by default) can be activated via: 
<bash>/mcDet/setIsZeroMagField true 
</bash></p>

<p>In Geant4 VMC, user can customize the default integration method and the default accuracy parameters with a set of dedicated commands. It is a user responsibility to choose the type of equation of motion and the integration method compatible with the user field. (The defined commands are not limited to magnetic field only in order to make easier an eventual future extension of VMC to electric and electromagnetic fields.)</p>

<p>The available commands:</p>

<ul>
	<li> <bash>
/mcMagField/stepperType stepperType </bash> 
       Selects the integrator (stepper) of particle's equation of motion; the default stepper is G4ClassicalRK4.</li>
	<li> <bash>
/mcMagField/equationType eqType </bash>
	Selects the type of equation of motion of a particle in a field; the default is G4Mag_UsualEqRhs.</li>
	<li>
	<bash>
/mcMagField/setDeltaIntersection value [unit]
/mcMagField/setDeltaOneStep value [unit]</bash>
	These commands set the accuracy parameters delta intersection and delta one step of the G4 Field Manager. Their default values in Geant4 are 0.001*mm and 0.01*mm respectively.</li>
	<li>
	<bash>
/mcMagField/setStepMinimum value [unit]
/mcMagField/setDeltaChord  value [unit]</bash>
	These commands set the step minimum and delta chord parameters to G4 Chord Finder. Their default values in Geant4 are 0.01*mm and 0.25*mm respectively.</li>
	<li>
	<bash>
/mcMagField/setMinimumEpsilonStep value
/mcMagField/setMaximumEpsilonStep value </bash>
	These commands set the MinimumEpsilonStep and the MaximumEpsilonStep parameters of the G4 Field Manager. Their default values in Geant4 are 5.0e-5 and 0.001 respectively.</li>
	<li>
	<bash>
/mcMagField/setConstDistance value [unit] </bash>
	This command (available since version 3.2) activates utilizing of a "cached magnetic field". The cached field value (the value from a previous call) is returned in case a new call is performed for a point which distance from a previous one is smaller than the value of the "ConstDistance" parameter.</li>
</ul>

<p>To customize local field parameters, a command directory for the local field has to be created first using:</p>

<ul>
	<li>
	<bash>
/mcDet/createMagFieldParameters volumeName</bash>
	</li>
</ul>
The same commands as for a global field are then available under this directory:

<ul>
	<li>
	<bash>
/mcMagField/volumeName/stepperType stepperType
/mcMagField/volumeName/equationType eqType
...</bash>
	</li>
</ul>

<p>How to apply Geant4 commands in a Root user session is explained at the section on <a href="/drupal/content/user-interfaces"> User Interfaces</a>.</p>

<p>Since Geant4 VMC version 3.2, users can also provide their own magnetic field equation of motion and/or its integrator. These objects should be instantiated in a detector construction class derived from <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4DetConstruction.html"> TG4DetConstruction</a>. This use case is demonstrated in the E03 example in <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03G4DetectorConstruction.html> Ex03G4DetectorConstruction</a> and <a href="http://ivana.home.cern.ch/ivana/examples_html/classEx03RunConfiguration4.html"> Ex03RunConfiguration4</a> classes. How to include user Geant4 classes in a VMC application is explained at the section on  <a href="/drupal/content/user-geant4-classes> User Geant4 classes</a>.


