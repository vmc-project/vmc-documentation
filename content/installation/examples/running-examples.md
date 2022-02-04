+++
title = "Running examples"
menuTitle = ""
chapter = false
weight = 1
+++

First, make sure that you have included all libraries paths in your shared library path.

For all MCs:
```text
/your_path/root_install/lib
/your_path/vmc_install/lib
```

For Geant3 only:
```text
path to Pythia6 library
/your_path/geant3_install/lib[64]
/your_path/examples_install_g3/lib[64]
```

For Geant4 only:
```text
/your_path/geant4_install/lib[64]
/your_path/geant4_vmc_install/lib[64]
/your_path/examples_install_g4/lib[64]
```

#### Running examples from Root session

The example can be run by calling the provided macros from Root session:
```bash
$ cd geant4_vmc/examples/E01
$ root
root[0] .x load_g4.C   # load all libraries needed to run with Geant4
root[1] .x run_g4.C    # run with Geant4
or
root[0] .x load_g3.C   # load all libraries needed to run with Geant3
root[1] .x run_g3.C    # run with Geant3
```

#### Running examples application programs

The example can be also run by calling the executable from the examples installation directory:
```bash
$ cd geant4_vmc/examples/E01
$ /mypath/examples_install_g4/bin/g4vmc_exampleE01
```

For keeping maximum simplicity of the code, a fixed configuration is defined in the examples `main()` function. More flexible version of the `main()` function is provided in the test programs which default configuration options can be changed by running the program with selected command line options, eg.
```bash
$ g4vmc_testE01
   [-g4g,  --g4-geometry]         Geant4 VMC geometry option
   [-g4pl, --g4-physics-list]     Geant4 physics list selection
   [-g4sp, --g4-special-physics]  Geant4 special physics selection
   [-g4m,  --g4-macro]            Geant4 macro
   [-g4vm, --g4-vis-macro]        Geant4 visualization macro
   [-r4m,  --root-macro]          Root macro
   [-v,    --verbose]             verbose option (yes,no)
```
or  
```bash
$ g3vmc_testE01
   [-g3g,  --g3-geometry]         Geant3 geometry option    
   [-r4m,  --root-macro]          Root macro
   [-v,    --verbose]             verbose option (yes,no)
```

Note that the g4* and g3* options are available only when the program was built with the corresponding `VMC_WITH_Geant4` or `VMC_WITH_Geant3` option. Root macro with arguments has to be passed in a single string, eg.:
```bash
   --root-macro 'test_E01.C("",kFALSE)'
```
