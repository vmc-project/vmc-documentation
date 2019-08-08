+++
title = "Special Cuts and Regions"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 7
+++

<p>
The way of applying cuts is different in Geant3 and Geant4.
While in Geant3, the cuts are defined as a limit in energy,
which is applied both as an energy threshold (a secondary particle is not produced if its energy is beyond the threshold) and a tracking cut (a particle is stopped when its energy gets below the cut), in Geant4, there is defined a unique cut in range which
is then converted to an energy threshold per particle and material.
The advantage is that you keep the same spatial resolution
of your energy deposit over the whole detector. It is also possible to define cuts per regions, as in big experimental setups you may want to speed up your simulation by setting a higher cut in the support structures etc. See more details at the Geant4 User 
Guide for Application Developers, <a href="http://geant4.web.cern.ch/geant4/UserDocumentation/UsersGuides/ForApplicationDeveloper/html/ch05s04.html"> section 5.4. Production Threshold versus Tracking</a>.

<h3> VMC cuts </h3>
<p>
The VMC provides a possibility for a user to define cuts in Geant3 way. The cuts can be defined globally or per tracking medium via the following <a href="http://root.cern.ch/root/htmldoc/TVirtualMC.html"> TVirtualMC </a> functions:
<cpp>gMC->SetCut(cutname, cutValue);
gMC->Gstpar(medId, cutName, cutValue);
</cpp>

The user defined VMC and their interpretation in Geant4 can be viewed with the following commands: <br>
To print the cut values for the given cut type (cutName) and control values for the given control type (controlName) for all tracking media:
<bash>/mcDet/printCuts  cutName
/mcDet/printControls  controlName
</bash>

To print global cuts and global process controls:
<bash>/mcPhysics/printGlobalCuts
/mcPhysics/printGlobalControls
</bash>

To print the user limits (including the VMC cuts and controls) set 
for the specified volume:
<bash>/mcPhysics/printVolumeLimits volName
</bash>

<h3> Geant4 cuts </h3>
<p>
<i>By default, Geant4 VMC ignores the VMC cuts</i> and applies only the global cut in range defined in the physics list. The default global cut value in Geant4 VMC is 1*mm for all particles which cut is applied for (gamma, e-, e+). User can change this default value for each particle separately or set a new value for all using the commands:
<bash>/mcPhysics/rangeCutForGamma    value    
/mcPhysics/rangeCutForElectron value
/mcPhysics/rangeCutForPositron value
/mcPhysics/rangeCuts value
</bash>

<h3> Applying VMC cuts in Geant4 </h3>

In order to take the VMC cuts into account, the user has to activate the special cuts process by switching it on in his g4Config.C (see the section on <a href="http://root.cern.ch/physics-list"> Physics list selection </a>). This special cuts process applies the VMC cuts as tracking cuts using G4UserLimits.

<p>
When the special cuts process is activated, Geant4 VMC defines also regions according to the VMC cuts defined by the user. The regions apply the VMC cuts as an energy threshold. In order to do this, the cut energy has to be converted in range. This conversion is performed in <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4RegionsManager.html"> TG4RegionsManager </a> using the Geant4 converter classes G4RToEConvForElectron and G4RToEConvForGamma by iterating within a given range interval up to a given precision. User can change the default precision (2 orders of magnitude) with the command:
<bash>/mcRegions/setRangePrecision value
</bash>

<p>
The conversion of VMC cuts to the regions follows these rules:
<ul>
<li> If the VMC energy cut defined by user results in a range cut smaller than the default range cut value defined in user physics list, the VMC cut is ignored and the default range cut is used.

<li> The range cut is first evaluated within the range 1e-03mm to   1m; when the range cut order is found, it is refined up to given precision (2 orders of magnitude by default) within 10 values of each order and the range value with the closest energy still smaller than VMC cut is chosen.<br>
It may happen that a value cannot be refined up to given precision,
then the best found value is returned.

<li> The regions are defined only in case when the VMC cuts result in range cuts different from the range cuts in the default region; then the region includes all logical volumes with a given material.

<li> As the range cuts do not match precisely to user defined energy cuts, the specialCuts process applies the energy cuts as tracking cuts when a particle with energy cut below threshold is generated.
</ul>

<p>
User can select several levels of verbosity to control the process of regions definition:
<bash>/mcVerbose/regionsManager level   
  level = 0  no output
          1  number of regions added via VMC
          2  the list of all volumes, the cuts in energy and
             calculated cuts in range
          3  all evaluated energy values
</bash>

<p>
It is also possible to dump the regions properties for a specified volume or to activate printing properties of all regions:
<bash>/mcRegions/dump volumeName
/mcRegions/print true|false
</bash>

<p>
How to apply Geant4 commands in a Root user session is explained at the section on <a href="vmc-user-interfaces"> User Interfaces</a>.
