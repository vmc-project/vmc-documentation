+++
title = "Installing and Running Examples"
menuTitle = "Examples"
chapter = false
hidden = false
showhidden = true
weight = 3
+++

<h3> Building examples </h3>

<p>
The following instructions apply to the installation since the version 3.00. Since this 3.00, VMC examples are installed with CMake. 

<p>
The VMC examples libraries require ROOT installation, the VMC examples programs can be built against Geant3 with VMC or Geant4 VMC libraries.

<p>
By default, the VMC examples libraries and the VMC examples programs are built against Geant4 VMC libraries together with Geant4 VMC installation. Below we provide the instructions how to build VMC examples altogether outside Geant4 VMC. The analogous instructions can be used to build each example individually or to build a user application. 

<p>
To install all examples
<ol>
    <li> First get the Geant4 VMC source from the 
    <a href="/drupal/content/download-vmc">Download page</a>, as the examples are provided within this package.
    We will assume that the Geant4 VMC package sits in a subdirectory
<bash>
/mypath/geant4_vmc
</bash>
    <li> Create build directory alongside our source directory for installation with Geant4:
<bash>
$ cd /mypath
$ mkdir examples_build_g4
$ ls
geant4_vmc examples_build_g4    
</bash>
and/or for installation with Geant3:
<bash>
$ cd /mypath
$ mkdir examples_build_g3
$ ls
geant4_vmc examples_build_g3    
</bash>
and/or for the installation without specific MC (only libraries will be installed in this case):
<bash>
$ cd /mypath
$ mkdir examples_build
$ ls
geant4_vmc examples_build    
</bash>
    <li> To configure the build, change into the build directory and run CMake with either -DVMC_WITH_Geant4 
<bash>
$ cd /mypath/example_build_g4 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install_g4 \
    -DVMC_WITH_Geant4=ON \
    -DGeant4VMC_DIR=/mypath/geant4_vmc_install/lib[64]/Geant4VMC-3.0.0 \  
    /mypath/geant4_vmc/examples
</bash>
or -DVMC_WITH_Geant3.
<bash>
$ cd /mypath/example_build_g3 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install_g3 \
    -DVMC_WITH_Geant3=ON \
    -DGeant3_DIR=/mypath/geant3_install/lib[64]/Geant3-2.0.0 \
    -DPythia6_LIB_DIR=/my_path_to_pythia6_library \ 
    /mypath/geant4_vmc/examples
</bash>
or no specific MC:
<bash>
$ cd /mypath/example_build_g3 
$ cmake -DCMAKE_INSTALL_PREFIX=/mypath/examples_install \
    -DCMAKE_MODULE_PATH=/mypath/geant4_vmc_install/lib[64]/Geant4VMC-3.0.0/Modules \  
    /mypath/geant4_vmc/examples
</bash>
<p> Note that in the last case, you still have to define the path to CMake configuration files used in examples which are provided with both Geant4 VMC and Geant3 CMake installation. Only examples libraries are built and installed in this case.
<p> If ROOT and Geant4 environment was defined using thisroot.&#91;c&#93;sh and geant4.&#91;c&#93;sh scripts, there is no need to provide the path to their installations. Otherwise, they can be provided using -DROOT_DIR and -DGeant4_DIR cmake options. 
</p>
   <li> After the configuration has run, CMake will have generated Unix Makefiles for building examples. To run the build, simply execute make in the build directory:
<bash>
$ make -jN
</bash>
where N is the number of parallel jobs you require (e.g. if your machine has a dual core processor, you could set N to 2).
<p>
If you need more output to help resolve issues or simply for information, run make as
<bash>
$ make -jN VERBOSE=1
</bash>

<li> Once the build has completed, you can install examples to the directory you specified earlier in CMAKE_INSTALL_PREFIX by running
<bash>
$ make install
</bash>
</ol>
This will install examples libraries in lib[64] and executables in bin directory in CMAKE_INSTALL_PREFIX directory.

<h4> VMC Examples Build Options </h4>

<p>
Overview of available options and their default values:
<bash>
VMC_WITH_Geant4       Build with Geant4   OFF
VMC_WITH_Geant3       Build with Geant3   OFF
VMC_WITH_MTRoot       Build with MTRoot   ON
VMC_INSTALL_EXAMPLES  Install examples libraries and programs  ON
</bash>

<p> Geant4 VMC build options are automatically exported in Geant4VMCConfig.cmake. If Geant4 VMC  was built with VGM, the VGM installation path has to be provided via -DVGM_DIR option.
<p>

<h3> Running examples </h3>

<p>
First, make sure that you have included all libraries paths in your shared library path. <br />

<p>
For all MCs:<br />
<bash>
/your_path/root/lib
/your_path/mtroot_install/lib[64]  
</bash>
The MTRoot package is provided in Geant4 VMC and is installed by default with Geant4 VMC library and is located in geant4_vmc_install. It can be however installed also stand-alone and without Geant4 libraries.

<p> 
For Geant3 only:<br />
<bash>
path to Pythia6 library
/your_path/geant3_install/lib[64]
/your_path/examples_install_g3/lib[64]
</bash>

<p>
For Geant4 only:<br />
<bash>
/your_path/geant4_install/lib[64]
/your_path/geant4_vmc_install/lib[64]
/your_path/examples_install_g4/lib[64]
</bash>

<h4> Running examples from Root session </h4>

<p>
The example can be run by calling the provided macros from Root session: <br>
<bash>
$ cd geant4_vmc/examples/E01
$ root
root[0] .x load_g4.C   # load all libraries needed to run with Geant4
root[1] .x run_g4.C    # run with Geant4
or
root[0] .x load_g3.C   # load all libraries needed to run with Geant3
root[1] .x run_g3.C    # run with Geant3
</bash>
</p>

<h4> Running examples application programs </h4>

<p>
The example can be also run by calling the executable from the examples installation directory: <br>
<bash>
$ cd geant4_vmc/examples/E01
$ /mypath/examples_install_g4/bin/g4vmc_exampleE01
</bash>
</p>

<p>  
For keeping maximum simplicity of the code, a fixed configuration is defined in the examples main() function. More flexible version of the main() function is provided in the test programs which default configuration options can be changed by running the program with selected command line options, eg.
<bash>
$ g4vmc_testE01
   [-g4g,  --g4-geometry]         Geant4 VMC geometry option
   [-g4pl, --g4-physics-list]     Geant4 physics list selection
   [-g4sp, --g4-special-physics]  Geant4 special physics selection
   [-g4m,  --g4-macro]            Geant4 macro
   [-g4vm, --g4-vis-macro]        Geant4 visualization macro
   [-r4m,  --root-macro]          Root macro
   [-v,    --verbose]             verbose option (yes,no)
</bash>
or  
<bash>
$ g3vmc_testE01
   [-g3g,  --g3-geometry]         Geant3 geometry option    
   [-r4m,  --root-macro]          Root macro
   [-v,    --verbose]             verbose option (yes,no)
</bash>

<p>
Note that the g4* and g3* options are available only when the program was built with the corresponding VMC_WITH_Geant4 or VMC_WITH_Geant3 option. Root macro with arguments has to be passed in a single string, eg.:
<bash>
   --root-macro 'test_E01.C("",kFALSE)'
</bash>
</p>

For the old instructions see:

- [Older Versions](examples-old)


