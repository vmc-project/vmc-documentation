+++
title = "VMC Project"
chapter = false
+++

# VMC Project

**Virtual Monte Carlo** (VMC) defines an abstract layer between a detector simulation user code (MC application) and the Monte Carlo transport code (MC). In this way the user code is independent of any specific MC and can be used with different transport codes within the same simulation application. 

The implementation of the interface is provided for two Monte Carlo transport codes, GEANT3 and [Geant4](http://geant4.web.cern.ch/geant4/). The implementation for the third Monte Carlo transport code, FLUKA, has been discontinued by the FLUKA team in 2010.

VMC was developed by the [ALICE Software Project](http://aliceinfo.cern.ch/Offline/) and, after the complete removal of all dependencies from the experiment specific framework, it was included in [ROOT](https://root.cern.ch/) and then gradually separated from ROOT into a stand-alone [vmc-projet](https://github.com/vmc-project).

These are new documentation pages, migrated from [root.cern.ch/vmc](https://root.cern.ch/vmc). If you have suggestions about how to improve this documentation, you can let us know. See [Support](/support).

{{% notice info %}}
**Reference paper**\
Hřivnáčová I et al: The Virtual MonteCarlo,\
ECONF C0303241:THJT006,2003; [e-Print: cs.SE/0306005](https://arxiv.org/abs/cs/0306005)
{{% /notice %}}

<i class="far fa-envelope"></i> Contact: <a href="mailto: root-vmc@cern.ch"> root-vmc@cern.ch</a>

*Last update: 21/07/2020*
