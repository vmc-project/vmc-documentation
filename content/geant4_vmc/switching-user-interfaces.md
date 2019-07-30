+++
title = "Switching User Interfaces"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 10
+++

<p>The VMC interface provides a common denominator for all implemented MC's and cannot cover all commands available in a Geant4 user session through Geant4 user interface (UI). Switching between the Root UI and the Geant4 UI gives a user the possibility of working with the native Geant4 UI. It is also possible to process a foreign command or a foreign macro in both UIs:</p>

<h3>From Root to Geant4 UI </h3>
<p>Switching UI: </p>
<cpp>root [0] ((TGeant4*)gMC)->StartGeantUI();
</cpp>
<p>Call Geant4 macro "myMacro.in" from Root: </p>
<cpp>root [0] ((TGeant4*)gMC)->ProcessGeantMacro("myMacro.in");
</cpp>
<p>Call Geant4 command from Root:</p>
<cpp>root [0] ((TGeant4*)gMC)->ProcessGeantCommand("/tracking/verbose 1");
</cpp>    

<h3>From Geant4 to Root UI </h3>
<p>Switching UI: </p>
<bash>Idle> /mcControl/root
</bash>
<p>Call Root macro "myMacro.C" from Geant4: </p>
<bash>Idle> /mcControl/rootMacro myMacro
</bash>     
<p>Call Root command from Geant4:</p>
<bash>Idle> /mcControl/rootCmd TBrowser b;
</bash>      

<h3>Geant4 VMC commands </h3>
<p>Geant4 VMC implements several Geant4 commands associated with the objects defined in Geant4 VMC. To make their Geant4 VMC origin apparent, all these commands start with the prefix <i> mc </i>.  Several Geant4 and Geant VMC commands are used in the    <i> g4config.in </i> and  <i> g4vis.in </i> macros in the  <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html">  VMC examples </a>.</p>
<p>You can get an interactive help for all available commands by typing:</p>
<bash>Idle> help
</bash>
or
<bash>Idle> /control/help
</bash>
