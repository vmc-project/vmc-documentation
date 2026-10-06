+++
title = "VMC Examples"
menuTitle = ""
chapter = false
weight = 1
#pre = "<b>4. </b>"
+++

A set of examples of usage of VMC is included in **geant4_vmc**. 

Though provided within Geant4 VMC, they can be built independently and do not require Geant4 installation in case you want to run them with GEANT3 only.

See more details on the [VMC Examples](https://vmc-project.github.io/geant4_vmc/examples_html/) web page.


### E03d: RNTuple output (development version)

The development version after Geant4 VMC 6.8 adds E03d, adapted from E03a, to demonstrate RNTuple output, including parallel writing. It is not part of release 6.8. The development CMake configuration enables it for ROOT versions strictly newer than 6.38.00.

From the E03 directory, load E03d in ROOT and select the sequential RNTuple macro:

```
root[0] .x load_g4d.C
root[1] .x run_g4_rntuple.C
```

For parallel writing, use `run_g4_rntuple_parallel.C` instead. See the E03 README supplied with the development source for the available output modes and run instructions.
