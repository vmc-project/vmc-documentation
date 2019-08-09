+++
title = "Switching User Interfaces"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 10
+++

The VMC interface provides a common denominator for all implemented MC's and cannot cover all commands available in a Geant4 user session through Geant4 user interface (UI). Switching between the Root UI and the Geant4 UI gives a user the possibility of working with the native Geant4 UI. It is also possible to process a foreign command or a foreign macro in both UIs:

### From Root to Geant4 UI

Switching UI:
```cpp
root [0] ((TGeant4*)gMC)->StartGeantUI();
```

Call Geant4 macro `myMacro.in` from Root:
```cpp
root [0] ((TGeant4*)gMC)->ProcessGeantMacro("myMacro.in");
```

Call Geant4 command from Root:
```cpp
root [0] ((TGeant4*)gMC)->ProcessGeantCommand("/tracking/verbose 1");
```

### From Geant4 to Root UI

Switching UI:
```bash
Idle> /mcControl/root
```

Call Root macro "myMacro.C" from Geant4:
```bash
Idle> /mcControl/rootMacro myMacro
```

Call Root command from Geant4:
```bash
Idle> /mcControl/rootCmd TBrowser b;
```

### Geant4 VMC commands

Geant4 VMC implements several Geant4 UI commands associated with the objects defined in Geant4 VMC. To make their Geant4 VMC origin apparent, all these commands start with the prefix <i>mc</i>.  Several Geant4 and Geant VMC commands are used in the `g4config.in` and `g4vis.in`  macros in the  <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html">  VMC examples </a>.

You can get an interactive help for all available commands by typing:
```bash
Idle> help
```
or
```bash
Idle> /control/help
```
