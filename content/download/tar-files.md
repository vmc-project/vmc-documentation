+++
title = "Tar Files"
menuTitle = ""
chapter = false
weight = 5
#pre = "<b>1. </b>"
+++

### Pro versions

| Package | Version | Tar file | Tested with |
|---------|---------|----------| ------------|
| vmc | 1.0 | [v1-0.tar.gz](https://github.com/vmc-project/vmc/archive/v1-0.tar.gz) | *ROOT 6.18/04 (\*)* |
| geant3 | 3.0 | [v3-0.tar.gz](https://github.com/vmc-project/geant3/archive/v3-0.tar.gz) | *ROOT 6.18/04 (\*)* |
| geant4_vmc | 5.1 | [v5-1.tar.gz](https://github.com/vmc-project/geant4_vmc/archive/v5-1.tar.gz) | *ROOT 6.20.00*,<br> *Geant4 10.6.p01 (with embedded CLHEP 2.4.1.3),* <br> *VGM 4.8,* <br> *Garfield master at 940d64b1a5be99a64*|

*(\*)  VMC 1.0 is ahead of the vmc in ROOT 6.18/04. It includes fixes scheduled for the next ROOT tags 6.18/06 and 6.20/00.
  For this reason, the new multiple engine mode works correctly only with the VMC stand-alone or with vmc in ROOT master or v6-18-patches or v6-20-patches.*

In general, the VMC packages can be built with the Root versions which they were tested with and higher, and Geant4 VMC with the Geant4 version which it was tested with including the patches. Note that the Geant4 patches released after the Geant4 VMC tag do not appear in the table above, it is however recommended to update Geant4 with each patch release.

### Old versions

| Package | Version | Tar file | Tested with |
|---------|---------|----------| ------------|
| geant3 | 2.7 | [v2-7.tar.gz](https://github.com/vmc-project/geant3/archive/v2-7.tar.gz) | *ROOT 6.14.06*  |
