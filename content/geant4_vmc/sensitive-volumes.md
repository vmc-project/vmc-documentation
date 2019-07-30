+++
title = "Sensitive Detectors and Volumes"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 3
+++

<h2>Sensitive Detectors</h2>

<p>Recently (since ROOT version v6.13/04) a new interface to user sensitive detector, <a href="https://root.cern.ch/doc/master/classTVirtualMCSensitiveDetector.html">TVirtualMCSensitiveDetector</a>, has been added in the set of VMC interfaces. The support fot this new way of settinv sensitive detector is available in geant3 and geant4_vmc development versions.</p>

<p>The user sensitive detectors object should be associated to the selected volumes in the new dedicated MCApplication function<br />
<cpp>void TVirtualMCApplication::SetSensitiveDetectors()</cpp><br />
using the new TVirtualMC function:<br />
<cpp>void TVirtualMC::SetSensitiveDetector(const TString&amp; volumeName, TVirtualMCSensitiveDetector* userSD);</cpp></p>

<p>Users can also choose whether scoring should be performed exclusively via sensitive detectors or via both sensitive detectors and MCApplication::Stepping() using the function<br />
<cpp>void VirtualMC::SetExclusiveSDScoring(Bool_t);</cpp><br />
If exclusive scoring is selected, the&nbsp;MCApplication::Stepping() is not called by MC.</p>

<p>To demonstrate the usage of this new interfaces the E03 example was split in two variants:</p>

<ul>
	<li><strong>E03a</strong> - scoring via sensitive volumes and MCApplication::Stepping (old way)</li>
	<li><strong>E03b</strong> - scoring via sensitive detectors derived from new TVirtualMCSensitiveDetector interface</li>
</ul>

<h2>Sensitive Volumes</h2>

<p>The VMC interfaces did&nbsp;not provided functions for a user selection of sensitive volumes and (unless the new sensitive detector framework is used) the user MCApplication::Stepping() is called in all volumes. In order to speed up simulation, in Geant4 VMC (since version 2.13) the user has a possibility to select sensitive volumes. If any selection is provided, the user MCApplication::Stepping() is called only from the selected sensitive volumes.</p>

<p>The selection of sensitive volumes can be done in two ways</p>

<ol>
	<li>Via the following Geant4 VMC command: <bash> /mcDet/addSDSelection volName1 [volName2 ...] </bash> The command can be applied more times, the new selection is each time added to the existing ones.<br />
	&nbsp;</li>
	<li>Via labeling volumes directly in TGeo geometry. In this case, user has to notify Geant4 VMC about using the sensitive volumes selection from TGeo by applying Geant4 VMC command: <bash> /mcDet/setSDSelectionFromTGeo true </bash> The volumes in TGeo geometry are set sensitive by setting the option "SV" to TGeoVolume objects: <cpp> TGeoVolume* myVolume = ... myVolume-&gt;SetOption("SV"); </cpp> User can also choose a different string than "SV" for sensitive volumes labeling. In this case, he has to notify Geant4 VMC about the label via the Geant4 VMC command: <bash> /mcDet/setSVLabel MyLabel </bash> Note that the option set via TGeoVolume::SetOption function is not persistent.</li>
</ol>

<div>&nbsp;</div>

