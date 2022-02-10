+++
title = "Multi-threaded Processing"
menuTitle = ""
chapter = false
hidden = false
showhidden = true
weight = 12
+++

### Geant4 VMC with Multi-threading Geant4

Since version 3.00, Geant4 VMC supports running Geant4 in multi-threading (MT) mode. The VMC application will run automatically in MT mode when Geant4 VMC is built against Geant4 MT. This default behaviour can be changed via the option specified with creating [TG4RunConfiguration](https://vmc-project.github.io/geant4_vmc/g4vmc_html/classTG4RunConfiguration.html) passed as the fifth argument in TG4RunConfiguration constructor. *(Note that the fourth argument, specialStacking option, cannot be omitted in this case.)*

The VMC application which has not been migrated to MT should be run in a sequential mode, either with Geant4 VMC built against Geant4 sequential libraries or with Geant4 VMC built against Geant4 MT libraries with disabled multi-threading mode in `TG4RunConfiguration`. Otherwise its run will stop with an exception.

As the VMC classes work as a factory for creating Geant4 application objects (user initialization and user action classes, sensitive detector classes etc.), the main VMC objects: `TGeant4` and `MCApplication` need to be created on both master and worker threads. Creating of all objects on worker threads is triggered from the Geant4 VMC classes. Users need just to implement new functions of `TVirtualMCApplication` which are then used to clone the application and its containing objects on workers:
{{< highlight cpp >}}
 // required for running in MT
 virtual TVirtualMCApplication* CloneForWorker() const;
 // optional
 virtual void InitForWorker() const;
 virtual void BeginRunOnWorker();
 virtual void FinishRunOnWorker();
 virtual void Merge(TVirtualMCApplication* localMCApplication);
{{< /highlight >}}

Overriding of `TVirtualMCApplication::CloneForWorker()` is required, 
implementation of the other functions is optional.

The default number of threads, defined in Geant4, can be changed by setting the environment variable:
```bash
export G4FORCENUMBEROFTHREADS=4
```
or
```bash
setenv G4FORCENUMBEROFTHREADS 4
```
In applications which do not use G4Root navigation, it can be also changed via Geant4 UI command:
```bash
/run/numberOfThreads 4 
``` 

### Migration of VMC application to Multi-threading

1. Implement the required MCApplication function for MT:
   {{< highlight cpp >}}
virtual TVirtualMCApplication* CloneForWorker() const;
   {{< /highlight >}}
   and, optionally also the other functions listed in the previous section.

2. The step 1. will also require to implement the constructors for cloning MC application, primary generator, sensitive detectors and eventually other classes instantiated in your application: 
   {{< highlight cpp >}}
MyClass(const MyClass& origin);
   {{< /highlight >}}
   In general, the objects which state is modified during event processing need to be created on workers while the objects which are used in read-only mode can be be shared. See the examples implementation as a guidance.

3. Replace your Root manager with `TMCRootManager`, provided in the `vmc` library, which implements in a thread-safe way the functions previously provided in the VMC examples in the `Ex02RootManager` class.

4. Carefully check thread-safety of your application code. Note that ROOT has to be initialized for multi-threading via calling 
   {{< highlight cpp >}}
TThread::Initialize();
   {{< /highlight >}}
   just at start of your Root macro or your application.

      In VMC examples, this call is executed either in g4libs.C macro or in the example main program.
  
### Implementation details & tips

- Dynamic loading of libraries requires to build Geant4 libraries 
  with `-ftls-model=global-init` compilation flag (which is different
  from the Geant4 default, `-ftls-model=initial-exec`) and the tests 
  show that it brings a performance penalty.

     That's why it is recommended to build the VMC application
  program linked with all libraries. Since Geant4 VMC 3.00 version, 
  the main functions are provided for all VMC examples together with 
  CMake configuration file for their build.

- Be careful to call `TVirtualMCApplication` standard constructor
  when cloning your application:
  {{< highlight cpp >}}
TVirtualMCApplication(origin.GetName(),origin.GetTitle()),
  {{< /highlight >}}

- Check the existence of `TMCRootManager` in `YourSD::Initialize()` as when running in MT mode the `TMCRootManager` is not instantiated on master but only on workers.
  {{< highlight cpp >}}
if ( TMCRootManager::Instance() ) Register();
  {{< /highlight >}}

- The `vmc` package provides a locking mechanism similar to
  `G4AutoLock`, which can be used in the user application for handling 
  the operations which are not thread-safe. (It is not demonstrated in 
  the VMC examples, as there is no such use case where locking
  would be needed.)
  {{< highlight cpp >}}
// An example of use TMCAutoLock in UserClass.cxx

// Define mutex in a file scope 
namespace {
  // Mutex to lock application when performing not thread-safe operation
  TMCMutex unsafeOperationMutex = TMCMUTEX_INITIALIZER;
}  

// In a function where unsafeOperation() is called
{ ...
  TMCAutoLock lm(&unsafeOperationMutex);
  unsafeOperation();
  lm.unlock();
  ...
}
  {{< /highlight >}}
