+++
title = "Stacking of Particles"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 5
+++

<p>The user  <a href="../../../../../../../root/htmldoc/TVirtualMCStack.html"> VMC stack </a> is used differently in Geant3 VMC and Geant4 VMC. Geant3 VMC pops both primary and secondary particles as they are provided by TVirtualMCStack::PopNextTrack(), while Geant4 VMC pops only primary particles using TVirtualMCStack::PopPrimaryForTracking() from the VMC stack. 

<p>
Stacking of secondary particles is then handled by Geant4 kernel and the user VMC stack only monitors this stacking. By default, Geant4 VMC saves each secondary particle when it starts its tracking (at the the pre-track phase). User can customize this default behaviour and choose also not to save secondary particles at all or to save them in the step of their parent particle, immediately after their creation.
This can be done using the command (see in the section on <a href="/drupal/content/user-interfaces"> User Interfaces</a> how to apply Geant4 commands in a Root user session):
<bash>/mcTracking/saveSecondaries selection
   selection = DoNotSave, SaveInPreTrack, SaveInStep
</bash>

<p>The consequence of this is that, by default, the particles added to the VMC stack in other than TVirtualMCApplication::GeneratePrimaries() functions are ignored in Geant4 tracking. User has to activate the stack popper special process (see <a href="http://root.cern.ch/drupal/content/physics-list-selection"> section on Physics list selection</a>), if he wants to add particles to the stack during tracking. The added particles are then handled as secondaries of the current track.</p>

<p>The stack classes in the VMC examples provide the same stacking mechanism for both Geant3 and Geant4 MCs and they are recommended to be used in a user application. <a name="Impl_SpecialCuts"></a></p>
