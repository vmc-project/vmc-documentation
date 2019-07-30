+++
title = "Building examples application programs - With 3.00.b01"
menuTitle = "Examples application programs - Old"
chapter = false
hidden = true
weight = 1
+++

<p>
The following instructions apply only to 3.00.b01 (beta) version and are kept for reference purpose only. For the production version (3.0) see  <a href="/drupal/content/installing-and-running-examples"> Installing and Running Examples</a> page.

<p>
Since Geant4 VMC 3.0.x, there are also provided examples main programs
which can be built with either shared or static libraries of all packages. Building with static libraries can make execution faster, especially when running in multi-threaded mode, than when running with dynamic loading of shared libraries in a "traditional" way.
</p>

<p>
The programs can be built using CMake in a standard way. The CMake configuration files, CMakeLists.txt, provided within examples are defined with use of a generic configuration files UseVMC.cmake and FindVMC.cmake,
provided in geant4_vmc/cmake directory together with other Find modules for all used packages. FindVMC.cmake is used to find all used packages which selection is made by the user via the CMake options defined in this file.
</p>

<p> Build with Geant4: <br>
<bash>
cd geant4_vmc
mkdir examples_build_g4
cd examples_build_g4
cmake \
  -DGeant4VMC_DIR=/my_path/geant4_vmc \
  -DWITH_GEANT4=ON \
  -DWITH_MTROOT=ON \
  -DWITH_VGM=ON \
  ../examples
make -jN
</bash>
</p>

<p> Build with Geant3: <br>
(<i>Note that WITH_GEANT4 option has to be set OFF explicitly, as its default is ON.</i>)<br>
<bash>
cd geant4_vmc
mkdir examples_build_g3
cd examples_build_g3
cmake \
  -DGeant4VMC_DIR=/my_path/geant4_vmc \
  -DGeant3VMC_DIR=/my_path/geant3 \
  -DWITH_GEANT4=OFF \
  -DWITH_GEANT3=ON \
  -DWITH_MTROOT=ON \
  ../examples
make -jN
</bash>
</p>

<p>
The example can be then run by calling the executable in the build directory: <br>
<bash>
cd geant4_vmc/examples/E01
../../examples_build_g4/E01/exampleE01
</bash>
</p>

<p>  
For keeping maximum simplicity of the code, a fixed configuration is defined in the examples main() function. More flexible version of the main() function is provided in the test programs which default configuration options can be changed by running the program with selected command line options, eg.
<bash>
  testE01
   [-g4g,  --g4-geometry]         Geant4 VMC geometry option
   [-g4pl, --g4-physics-list]     Geant4 physics list selection
   [-g4sp, --g4-special-physics]  Geant4 special physics selection
   [-g4m,  --g4-macro]            Geant4 macro
   [-g4vm, --g4-vis-macro]        Geant4 visualization macro
   [-g3g,  --g3-geometry]         Geant3 geometry option    
                                  (TGeant3,TGeant3TGeo)
   [-r4m,  --root-macro]          Root macro
   [-v,    --verbose]             verbose option (yes,no)
</bash>

<p>
Note that the g4* and g3* options are available only when the program was built with the corresponding WITH_GEANT3 or WITH_GEANT3 option. Root macro with arguments has to be passed in a single string, eg.:
<bash>
   --root-macro 'test_E01.C("",kFALSE)'
</bash>
</p>


