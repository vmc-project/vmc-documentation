+++
title = "Stacking of Particles"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 5
+++

The user [VMC stack](https://vmc-project.github.io/vmc/html/classTVirtualMCStack.html) is used differently in Geant3 VMC and Geant4 VMC. Geant3 VMC pops both primary and secondary particles as they are provided by `TVirtualMCStack::PopNextTrack()`, while Geant4 VMC pops only primary particles using `TVirtualMCStack::PopPrimaryForTracking()` from the VMC stack. 

Stacking of secondary particles is then handled by Geant4 kernel and the user VMC stack only monitors this stacking. By default, Geant4 VMC saves each secondary particle when it starts its tracking (at the the pre-track phase). User can customize this default behaviour and choose also not to save secondary particles at all or to save them in the step of their parent particle, immediately after their creation.
This can be done using the command (see in the section on  [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces) how to apply Geant4 commands in a Root user session):
```bash
/mcTracking/saveSecondaries selection
   selection = DoNotSave, SaveInPreTrack, SaveInStep
```

The consequence of this is that, by default, the particles added to the VMC stack in other than `TVirtualMCApplication::GeneratePrimaries()` functions are ignored in Geant4 tracking. Users have to activate the stack popper special process (see the section on [Physics list selection](/user-guide/geant4_vmc/physics-lists)), if they want to add particles to the stack during tracking. The added particles are then handled as secondaries of the current track.

The stack classes in the VMC examples provide the same stacking mechanism for both Geant3 and Geant4 MCs and they are recommended to be used in a user application.
