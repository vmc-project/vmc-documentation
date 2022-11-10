+++
title = "Sensitive Detectors and Volumes"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 3
+++

## Sensitive Detectors

Recently (since ROOT version v6.13.04) a new interface to user sensitive detector, [TVirtualMCSensitiveDetector](https://vmc-project.github.io/vmc/classTVirtualMCSensitiveDetector.html), has been added in the set of VMC interfaces. The support for this new way of definig sensitive detector is available since geant3 2.6 and geant4_vmc 4.0.

The user sensitive detectors object should be associated to the selected volumes in the new dedicated MCApplication function:
{{< highlight cpp >}}
void TVirtualMCApplication::SetSensitiveDetectors()
{{< /highlight >}}

using the new TVirtualMC function:
{{< highlight cpp >}}
void TVirtualMC::SetSensitiveDetector(
	const TString& volumeName,
	TVirtualMCSensitiveDetector* userSD);
{{< /highlight >}}
Users can also choose whether scoring should be performed exclusively via sensitive detectors or via both sensitive detectors and `MCApplication::Stepping()` using the function
{{< highlight cpp >}}
void VirtualMC::SetExclusiveSDScoring(Bool_t);
{{< /highlight >}}
If exclusive scoring is selected, the `MCApplication::Stepping()` is not called by MC.

To demonstrate the usage of this new interfaces the **E03 example** was split in two variants:

- **E03a** - scoring via sensitive volumes and `MCApplication::Stepping` (old way)
- **E03b** - scoring via sensitive detectors derived from new `TVirtualMCSensitiveDetector` interface

## Sensitive Volumes

The VMC interfaces did not provide functions for a user selection of sensitive volumes and (unless the new sensitive detector framework is used) the user `MCApplication::Stepping()` is called in all volumes. In order to speed up simulation, in Geant4 VMC (since version 2.13) the user has a possibility to select sensitive volumes. If any selection is provided, the user `MCApplication::Stepping()` is called only from the selected sensitive volumes.

The selection of sensitive volumes can be done in two ways:

1. Via the following Geant4 VMC command:
   ```
   /mcDet/addSDSelection volName1 [volName2 ...]
   ```
   The command can be applied more times, the new selection is each time added to the existing ones.

2. Via labeling volumes directly in TGeo geometry. In this case, user has to notify Geant4 VMC about using the sensitive volumes selection from TGeo by applying Geant4 VMC command: 
   ```
   /mcDet/setSDSelectionFromTGeo true
   ```
   The volumes in TGeo geometry are set sensitive by setting the option "SV" to TGeoVolume objects: 
   {{< highlight cpp >}}
TGeoVolume* myVolume = ...;
myVolume->SetOption("SV");
   {{< /highlight >}}
   User can also choose a different string than "SV" for sensitive volumes labeling. In this case, they have to notify Geant4 VMC about the label via the Geant4 VMC command: 
   ```
   /mcDet/setSVLabel MyLabel
   ```
   Note that the option set via `TGeoVolume::SetOption` function is not persistent.
