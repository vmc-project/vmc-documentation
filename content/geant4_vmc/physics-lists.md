+++
title = "Physics Lists"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 4
+++

<h2>Physics list selection</h2>

<h3>Physics</h3>

<p>Geant4 VMC does not provide any default physics list. User have to choose the physics list from the physics lists provided in Geant4 (see the Geant4 web pages on <a href="http://geant4.web.cern.ch/geant4/support/proc_mod_catalog/physics_lists/physicsLists.shtml"> Hadronic physics lists</a> and <a href="http://geant4.web.cern.ch/geant4/collaboration/working_groups/electromagnetic/physlist.shtml"> Electromagnetic physics builders</a> ) or include their own physics list. The choice of the physics list is done via the option specified with creating <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4RunConfiguration.html"> TG4RunConfiguration </a> object. This option is passed as <i>the second argument</i> in TG4RunConfiguration constructor (see <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html"> examples </a> for more details).</p>

<p>This option can be defined as: 
<bash> 
"<g4-physics-list>[_<EM-option>][+extra][+optical][+radDecay]" 
</bash> 
where</p>

<ul>
	<li> "&lt;g4-physics-list&gt;" - is a name of a Geant4 physics list; eg. FTFP_BERT, QGSP, ...<br />
	It is also possible to select "emStandard" which builds only G4EmStandardPhysics() and G4DecayPhysics()</li>
	<li>"&lt;_EM-option&gt;" - is the EM physics option ("none, _EMV, _EMX, _EMY, _LIV, _PEN")</li>
	<li>"+extra" - adds G4ExtraPhysics (in development version only)</li>
	<li>"+optical" - adds G4OpticalPhysics (in development version) or TG4OpticalPhysics (in version &lt;= 2.12)</li>
	<li>"+radDecay" - adds G4RadioactiveDecayPhysics (in development version only)</li>
</ul>

<p>Examples: 
<bash>"FTFP_BERT_EMV+optical" 
"QGSP_BIC_EMY+radDecay" 
</bash></p>

<p>The "&lt;g4-physics-list&gt;_&lt;_EM-option&gt;" must represent a valid Geant4 physics list (except for "emStandard"). Since Geant4 9.5 it is possible to combine all EM options with all physics lists.</p>

<h3>Special processes</h3>

<p>In order to activate the support of VMC features like the VMC cuts, VMC process controls, user has to activate the special processes defined in <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4SpecialPhysicsList.html">TG4SpecialPhysicsList</a>. The selection of special processes is passed as <i>third argument</i> in TG4RunConfiguration constructor (see <a href="http://ivana.home.cern.ch/ivana/examples_html/index.html"> examples </a> for more details):</p>
<bash>"[stepLimiter][+specialCuts][+specialControls][+stackPopper]" 
</bash> where

<ul>
	<li>"stepLimiter" - step limiter (default)</li>
	<li>"specialCuts" - VMC cuts</li>
	<li>"specialControls" - VMC controls for activation/inactivation selected processes</li>
	<li>"stackPopper" - stack popper physics</li>
</ul>
Example: 
<bash>"stepLimit+specialCuts" 
"stepLimit+specialCuts+stackPopper" 
</bash>

<p>&nbsp;</p>

<h3>Composed physics list</h3>

<p>According to the user selection, Geant4 VMC creates a <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4ComposedPhysicsList.html">TG4ComposedPhysicsList</a> object which is a composition of a selected Geant4 physics list (or a user physics list), the extra physics list ( <a href="http://ivana.home.cern.ch/ivana/g4vmc_html/classTG4ExtraPhysicsList.html">TG4ExtraPhysicsList</a> - in development version only) and the <a href="http://ivana.home.cern.ch/ivana/g4vmc_htm/classTG4SpecialPhysicsList.html"> TG4SpecialPhysicsList</a> object. The inclusion of the extra physics list is optional. The composed physics list processes the Geant4 (or user) physics list in the first order, then adds the extra physics processes (if selected) and finally the special processes, registered in TG4SpecialPhysicsList, are added in the last order.</p>

<p>In the development version (svn trunk), the user physics selection is added to the TGeant4 object title so that it can be retrievable in a user application.</p>

<h2>Inspecting particles and physics processes</h2>

<p>The instantiated particles and physics processes can be the viewed with the following Geant4 and Geant4 VMC commands.</p>

<p>To list all particles and processes: 
<bash>/particle/list 
/process/list 
/mcPhysics/dumpAllProcess 
</bash></p>

<p>To inspect a selected particle: 
<bash>/particle/select particleName 
/particle/process/dump 
/particle/property/dump 
/particle/property/decay/dump 
</bash></p>

<p>To print the mapping of G4 processes to VMC process codes and VMC (G3-like) process controls: 
<bash>/mcPhysics/printProcessMCMap 
/mcPhysics/printProcessControlMap 
</bash></p>

<h2>Electromagnetic physics</h2>

<p>Users can tailor the extra physics processes provided by Geant4 via the commands defined in the <font color="#aa0000" face="Courier"><code>/physics_lists/em/</code></font><font color="#aa0000" face="Courier"> </font> command directory and switch on the muon or gamma nuclear interaction, synchrotron radiation,  gamma or positron conversion to muon pair or positron conversion to hadrons processes, which are not which are not included by default:</p>
<bash>/physics_lists/em/SyncRadiation true|false
/physics_lists/em/SyncRadiationAll true|false
/physics_lists/em/GammaNuclear true|false 
/physics_lists/em/MuonNuclear true|false 
/physics_lists/em/GammaToMuons true|false 
/physics_lists/em/PositronToMuons true|false 
/physics_lists/em/PositronToHadrons true|false 
</bash>

<p>Users can also customize the parameters of the electromagnetic physics processes using the commands in the following directories (use interactive help to get more information): 
<bash>/process/eLoss/ 
/process/msc/ 
/process/em/ 
</bash></p>

<p>Since Geant4 VMC version 2.10, it is also possible to choose an extra model of energy loss, fluctuations or multiple scattering for selected tracking media and selected particles. This can be done via the following built-in commands: 
<bash>/mcPhysics/emModel/setEmModel PAI 
/mcPhysics/emModel/setRegions trackingMedium1 [trackingMedium2 ...] 
/mcPhysics/emModel/setParticles all 
</bash> 
The "setRegions" and "setParticles" commands must be followed by the list of tracking media names and particle names respectively, separated with a blank space. The "setParticles" command can also take as a parameter the keyword "all". The model will be then applied to all particles.</p>

<p>At present, the following models are supported: PAI and PAIPhoton models which are applied to ionisation process, and SpecialUrbanMsc, a special model tuned for ALICE EMCAL detector, which is applied to e- and e+ multiple scattering processes.</p>

<h2>Hadronic physics</h2>
User can activate a generation of the cross sections plots for a given projectile particle and a target element via the following commands: 
<bash>/mcCrossSection/makeHistograms [true|false] 
/mcCrossSection/setParticle proton 
/mcCrossSection/setElement Al
</bash> 

The default energy or momentum limits and the number of bins can be customised via the commands: 
<bash>/mcCrossSection/setMinKinE 1 MeV 
/mcCrossSection/setMaxKinE 1 TeV 
/mcCrossSection/setMinMomentum 10 MeV 
/mcCrossSection/setMaxMomentum 10 TeV 
/mcCrossSection/setNofBinsE 900 
/mcCrossSection/setNofBinsP 800
</bash> 

It is also possible to print the cross section value for a selected energy or a momentum for a selected cross section type (or all types) via the following commands:
 <bash>/mcCrossSection/setParticle anti_proton 
/mcCrossSection/setElement H 
/mcCrossSection/setMomentum 0.3 GeV 
/mcCrossSection/printCrossSection All 
</bash>

<p>How to apply Geant4 commands in a Root user session is explained at the section on <a href="/drupal/content/user-interfaces"> User Interfaces</a>.</p>

