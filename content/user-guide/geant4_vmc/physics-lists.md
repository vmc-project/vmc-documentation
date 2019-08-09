+++
title = "Physics Lists"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 4
+++

## Physics list selection

### Physics

Geant4 VMC does not provide a default physics list. User have to choose the physics list from the physics lists provided in Geant4 (see the [Geant4 Physics List Guide](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/PhysicsListGuide/html/index.html) or include their own physics list. The choice of the physics list is done via the option specified with creating <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4RunConfiguration.html"> TG4RunConfiguration </a> object. This option is passed as*the second argument* in `TG4RunConfiguration` constructor (see [examples](examples) for more details).</p>

This option can be defined as: 
```bash
"<g4-physics-list>[_<EM-option>][+extra][+optical][+radDecay]" 
```
where

- `<g4-physics-list>` - is a name of a Geant4 physics list; eg. FTFP_BERT, QGSP, ...<br />
   - It is also possible to select "emStandard" which builds only G4EmStandardPhysics() and G4DecayPhysics()
- `_<EM-option>` - is the EM physics option (none, _EMV, _EMX, _EMY, _LIV, _PEN)
- `+extra` - adds G4ExtraPhysics
- `+optical` - adds G4OpticalPhysics
- `+radDecay` - adds G4RadioactiveDecayPhysics 

Examples:
```bash
"FTFP_BERT_EMV+optical" 
"QGSP_BIC_EMY+radDecay" 
```

The `<g4-physics-list>_<EM-option>` must represent a valid Geant4 physics list (except for "emStandard"). Since Geant4 9.5 it is possible to combine all EM options with all physics lists.

### Special processes

In order to activate the support of VMC features like the VMC cuts, VMC process controls, user has to activate the special processes defined in <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4SpecialPhysicsList.html">TG4SpecialPhysicsList</a>. The selection of special processes is passed as the *third argument* in `TG4RunConfiguration` constructor (see <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html"> examples </a> for more details):
```bash
"[stepLimiter][+specialCuts][+specialControls][+stackPopper]" 
```
where

- `"stepLimiter"` - step limiter (default)
- `"specialCuts"` - VMC cuts
- `"specialControls"` - VMC controls for activation/inactivation selected processes
- `"stackPopper"` - stack popper physics

Examples:
```bash
"stepLimit+specialCuts" 
"stepLimit+specialCuts+stackPopper" 
```

### Composed physics list

According to the user selection, Geant4 VMC creates a <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4ComposedPhysicsList.html">TG4ComposedPhysicsList</a> object which is a composition of a selected Geant4 physics list (or a user physics list), the extra physics list ( <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4ExtraPhysicsList.html">TG4ExtraPhysicsList</a> - in development version only) and the <a href="http://ivana.home.cern.ch/ivana/g4vmc_htm/classTG4SpecialPhysicsList.html"> TG4SpecialPhysicsList</a> object. The inclusion of the extra physics list is optional. The composed physics list processes the Geant4 (or user) physics list in the first order, then adds the extra physics processes (if selected) and finally the special processes, registered in TG4SpecialPhysicsList, are added in the last order.

Since version 2.13, the user physics selection is added to the TGeant4 object title so that it can be retrievable in a user application.

## Inspecting particles and physics processes

The instantiated particles and physics processes can be the viewed with the following Geant4 and Geant4 VMC commands.

To list all particles and processes: 
```bash
/particle/list 
/process/list 
/mcPhysics/dumpAllProcess 
```

To inspect a selected particle: 
```bash
/particle/select particleName 
/particle/process/dump 
/particle/property/dump 
/particle/property/decay/dump 
```

To print the mapping of G4 processes to VMC process codes and VMC (G3-like) process controls: 
```bash
/mcPhysics/printProcessMCMap 
/mcPhysics/printProcessControlMap 
```

## Electromagnetic physics

Users can tailor the extra physics processes provided by Geant4 via the commands defined in the `/physics_lists/em/` command directory and switch on the muon or gamma nuclear interaction, synchrotron radiation,  gamma or positron conversion to muon pair or positron conversion to hadrons processes, which are not which are not included by default:
```bash
/physics_lists/em/SyncRadiation true|false
/physics_lists/em/SyncRadiationAll true|false
/physics_lists/em/GammaNuclear true|false 
/physics_lists/em/MuonNuclear true|false 
/physics_lists/em/GammaToMuons true|false 
/physics_lists/em/PositronToMuons true|false 
/physics_lists/em/PositronToHadrons true|false 
```

Users can also customize the parameters of the electromagnetic physics processes using the commands in the following directories (use interactive help to get more information): 
```bash
/process/eLoss/ 
/process/msc/ 
/process/em/ 
```

Since Geant4 VMC version 2.10, it is also possible to choose an extra model of energy loss, fluctuations or multiple scattering for selected tracking media and selected particles. This can be done via the following built-in commands: 
```bash
/mcPhysics/emModel/setEmModel PAI 
/mcPhysics/emModel/setRegions trackingMedium1 [trackingMedium2 ...] 
/mcPhysics/emModel/setParticles all 
```

The `setRegions` and `setParticles` commands must be followed by the list of tracking media names and particle names respectively, separated with a blank space. The `setParticles` command can also take as a parameter the keyword `"all"`. The model will be then applied to all particles.

At present, the following models are supported: PAI and PAIPhoton models which are applied to ionisation process, and SpecialUrbanMsc, a special model tuned for ALICE EMCAL detector, which is applied to e- and e+ multiple scattering processes.

### Hadronic physics

User can activate a generation of the cross sections plots for a given projectile particle and a target element via the following commands:
```bash
/mcCrossSection/makeHistograms [true|false] 
/mcCrossSection/setParticle proton 
/mcCrossSection/setElement Al
```

The default energy or momentum limits and the number of bins can be customised via the commands: 
```bash
/mcCrossSection/setMinKinE 1 MeV 
/mcCrossSection/setMaxKinE 1 TeV 
/mcCrossSection/setMinMomentum 10 MeV 
/mcCrossSection/setMaxMomentum 10 TeV 
/mcCrossSection/setNofBinsE 900 
/mcCrossSection/setNofBinsP 800
```

It is also possible to print the cross section value for a selected energy or a momentum for a selected cross section type (or all types) via the following commands:
```bash
/mcCrossSection/setParticle anti_proton 
/mcCrossSection/setElement H 
/mcCrossSection/setMomentum 0.3 GeV 
/mcCrossSection/printCrossSection All 
```

How to apply Geant4 commands in a Root user session is explained at the section on [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces).
