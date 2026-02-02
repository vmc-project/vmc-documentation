+++
title = "Geant4 Scoring"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 4
+++

## Geant4 command line scoring

Since Geant4 VMC version 6.8, it is possible to activate Geant4 
command line scoring (see details in the [Geant4 Application developers Guide, 
section Command-based scoring](https://geant4-userdoc.web.cern.ch/UsersGuides/ForApplicationDeveloper/html/Detector/commandScore.html)).

The activation should be done via calling

{{< highlight cpp >}}
void TG4RunConfiguration::SetUseOfG4Scoring()
{{< /highlight >}}

in user `g4Config.C`.

The scoring (mesh, scorers etc.) can be then defined via Geant UI commands defined in '/score'
directory in `g4config.in`. The scoring data are then automatically saved for each mesh
in a `mesh_name.txt` file at the end of run

## User defined scoring function

User can also provide their scoreweight function that takes (pdg, ekin) as arguments
(defined via `TG4ScoreWeightCalculator`) via a new `TG4RunConfiguration` class`
setter:

{{< highlight cpp >}}
void  TG4RunConfiguration::SetScoreWeightCalculator(TG4ScoreWeightCalculator swc)
void TG4RunConfiguration::SetUseOfG4Scoring()
{{< /highlight >}}

The user function is then, in `TG4SDManager::LateInitialize()`, wrapped to `G4ScoreWeightCalculator`
function that takes (const G4Step*) as argument and is applied to all Geant4 scorers with activated score weighting.
