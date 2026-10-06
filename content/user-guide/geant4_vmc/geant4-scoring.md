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
command line scoring (see details in the Geant4 Application developers Guide,
[section Command-based scoring](https://geant4-userdoc.web.cern.ch/UsersGuides/ForApplicationDeveloper/html/Detector/commandScore.html)).

The activation should be done via calling

{{< highlight cpp >}}
void TG4RunConfiguration::SetUseOfG4Scoring()
{{< /highlight >}}

in the user `g4Config.C`.

The scoring (mesh, scorers etc.) can be then defined via Geant UI commands, available in the
'/score' directory, in `g4config.in`.

The scoring data will be then automatically saved for each mesh
in a `mesh_name.txt` file at the end of simulation run.

## User defined scoring function

Since Geant4 VMC 6.8, users can provide a score-weight calculator. This interface requires Geant4 11.4. The callback type is:

```cpp
using TG4ScoreWeightCalculator =
  std::function<Double_t(Int_t pdg, Double_t ekin)>;
```

It returns a `Double_t` weight for the particle PDG code and kinetic energy. The kinetic energy is passed directly from Geant4 in its internal energy units (MeV), without conversion to VMC's GeV convention. For an energy expressed in GeV, divide by `CLHEP::GeV` using Geant4's `G4SystemOfUnits.hh`.

Register the callback in `g4Config.C` through:

```cpp
void TG4RunConfiguration::SetScoreWeightCalculator(TG4ScoreWeightCalculator swc);
```

Also activate Geant4 scoring with `SetUseOfG4Scoring()` and enable score weighting for the desired scorers. The callback is wrapped as a Geant4 function taking `const G4Step*` and applied to scorers with weighting enabled.
