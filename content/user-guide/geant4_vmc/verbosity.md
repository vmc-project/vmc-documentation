+++
title = "Vebosity"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 13
+++

For many Geant4 VMC classes (like for Geant4 classes) the user can select a higher verbosity level and activate various printings which can help in understanding or debugging his application.

- {{< highlight bash >}}
/mcVerbose/all level
{{< /highlight >}}
   - Set the same verbose level (level >= 0) to all Geant4 VMC objects. If level = 0 no printing is issued, the higher the level is more printings will be issued.

- {{< highlight bash >}}
/mcVerbose/geometryManager 2
{{< /highlight >}}
   - Activate the debug printing from VGM geometry conversion (when geomRootToGeant4 option is selected)

- {{< highlight bash >}}
/mcVerbose/regionsManager 1 [2] [3]
{{< /highlight >}}
   - level=1: print the number of regions added via VMC
   - level=2: print also the list of all volumes, the cuts in energy and calculated cuts in range
   - level=3: print also all evaluated energy values

- {{< highlight bash >}}
/mcVerbose/composedPhysicsList 2 [3] [4]
{{< /highlight >}}
   Set the same verbose level to the registed user physics lists.
   - level=2: print the table of registered material/cuts couples (the range cuts and energy thresholds per material and particle)
   - level=3: print also info from G4VUserPhysicsList::BuildPhysicsTable for each constructed particle  
   - level=4: print also info from G4VRangeToEnergyConverter::Convert() for each material

- {{< highlight bash >}}
/mcVerbose/specialPhysicsList 1 [4]
{{< /highlight >}}
   - level=1: print info about constructed special processes
   - level=4: print also info from G4VRangeToEnergyConverter::Convert() for each material

- {{< highlight bash >}}
/mcVerbose/physicsExtDecayer 1 [2]
{{< /highlight >}}
   - level=1: print info about the constructed external decayer
   - level=2: print also info about the particles which default decay was disabled by user

- {{< highlight bash >}}
/mcVerbose/primaryGeneratorAction 2 
{{< /highlight >}}
   - Print the list of all primary particles for each event.

- {{< highlight bash >}}
/mcVerbose/runAction 1
{{< /highlight >}}
   - Print info at the start and the end of run including the time of the run.  

- {{< highlight bash >}}
/mcVerbose/eventAction 1 [2] [3]
{{< /highlight >}}
   - level=1: print info at the start of event and the info about number of trajectories in the event if storing trajectories is activated
   - level=2: print also info at the end of event including the time of the event
   - level=3: print also info about the number of primary tracks processed and total number of tracks saved in stack in each event

- {{< highlight bash >}}
/mcVerbose/trackingAction 2 [3]
{{< /highlight >}}
   - level=2: print info at the start of each 10th primary track
   - level=3: print also info at the start of each primary track

### Looping and problematic tracks </h3>

In order to avoid infinite looping of tracks, Geant4 VMC defines a maximum  number of steps allowed (the default value is 30000). User can customize this value by the command:
```bash
/mcTracking/maxNofSteps value
```

When a track reaches the maximum number of steps, Geant4 VMC activates the tracking verbose mode and let the track process a few more steps with printing the verbose info. This can help to identify the positon of the track in the geometry hierarchy. User can change this default behavior and set the desired level of verbosity:
```bash
/mcTracking/loopVerbose level
```
It is also possible to change the verbosity level for a selected track number using the commands:
```bash
/mcTracking/newVerboseTrack trackID
/mcTracking/newVerbose level
```

How to apply Geant4 commands in a Root user session is explained at the section on [Switching User Interfaces](/user-guide/geant4_vmc/switching-user-interfaces).
