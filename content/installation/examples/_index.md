+++
title = "Installing and Running Examples"
menuTitle = "Examples"
chapter = false
hidden = false
showhidden = true
weight = 3
+++


The following instructions apply to the installation since the version 3.00. Since this 3.00, VMC examples are installed with CMake. 

The VMC examples libraries require [ROOT](https://root.cern.ch/) installation, the VMC examples programs can be built against Geant3 with VMC or Geant4 VMC libraries.

By default, the VMC examples libraries and the VMC examples programs are built against Geant4 VMC libraries together with Geant4 VMC installation. Below we provide the instructions how to build VMC examples altogether outside Geant4 VMC. The analogous instructions can be used to build each example individually or to build a user application. 

To install all examples

1. First get the Geant4 VMC source from the [Download page](/download/geant4_vmc), as the examples are provided within this package. We will assume that the Geant4 VMC package sits in a subdirectory
{{< highlight bash >}}
/mypath/geant4_vmc
{{< /highlight >}}

2. Create build directory alongside our source directory for installation with Geant4:
{{< highlight bash >}}
$ cd /mypath
$ mkdir examples_build_g4
$ ls
geant4_vmc examples_build_g4    
{{< /highlight >}}
and/or for installation with Geant3:
{{< highlight bash >}}
$ cd /mypath
$ mkdir examples_build_g3
$ ls
geant4_vmc examples_build_g3    
{{< /highlight >}}
and/or for the installation without specific MC (only libraries will be installed in this case):
{{< highlight bash >}}
$ cd /mypath
$ mkdir examples_build
$ ls
geant4_vmc examples_build    
{{< /highlight >}}

3. To configure the build, change into the build directory and run CMake with either `-DVMC_WITH_Geant4` 
{{< highlight bash >}}
$ cd /mypath/example_build_g4
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install_g4 \
    -DVMC_WITH_Geant4=ON \
    -DGeant4VMC_DIR=/mypath/geant4_vmc_install/lib[64]/Geant4VMC-3.0.0 \
    /mypath/geant4_vmc/examples
{{< /highlight >}}
or `-DVMC_WITH_Geant3`,
{{< highlight bash >}}
$ cd /mypath/example_build_g3
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install_g3 \
    -DVMC_WITH_Geant3=ON \
    -DGeant3_DIR=/mypath/geant3_install/lib[64]/Geant3-2.0.0 \
    -DPythia6_LIB_DIR=/my_path_to_pythia6_library \
    /mypath/geant4_vmc/examples
{{< /highlight >}}
or no specific MC:
{{< highlight bash >}}
$ cd /mypath/example_build_g3
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install \
    -DCMAKE_MODULE_PATH=/mypath/geant4_vmc_install/lib[64]/Geant4VMC-3.0.0/Modules \  
    /mypath/geant4_vmc/examples
{{< /highlight >}}

   - Note that in the last case, you still have to define the path to CMake configuration files used in examples which are provided with both Geant4 VMC and Geant3 CMake installations. Only examples libraries are built and installed in this case.

   - If ROOT and Geant4 environment was defined using `thisroot.[c]sh` and `geant4.[c]sh` scripts, there is no need to provide the path to their installations. Otherwise, they can be provided using `-DROOT_DIR` and `-DGeant4_DIR` cmake options. 

4. After the configuration has run, CMake will have generated Unix Makefiles for building examples. To run the build, simply execute make in the build directory:
{{< highlight bash >}}
$ make -jN
{{< /highlight >}}
where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).

  - If you need more output to help resolve issues or simply for information, run make as
    {{< highlight bash >}}
    $ make -jN VERBOSE=1
    {{< /highlight >}}

5. Once the build has completed, you can install examples to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
{{< highlight bash >}}
$ make install
{{< /highlight >}}

This will install examples libraries in lib[64] and executables in bin directory in CMAKE_INSTALL_PREFIX directory.

{{% children  %}}

<hr>

For the old instructions see:

- [Older Versions](examples-old)
