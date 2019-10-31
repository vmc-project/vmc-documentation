+++
title = "Multiple VMC Engines"
menuTitle = "Multiple VMC"
chapter = false
weight = 3
#pre = "<b>3.1 </b>"
+++

## Introduction
The VMC package allows splitting the event simulation among multiple engines. The criteria of how to split the simulation have to be defined by the user and among others the decision which engine to be used could depend on

* geometry
* particle phase space
* particle type
* any combination of that
* ...

Running multiple engines is fully supported for

* [GEANT3_VMC](https://github.com/vmc-project/geant3)
* [GEANT4_VMC](https://github.com/vmc-project/geant4_vmc)

This also allows the user to implement his/her own class deriving from [TVirtualMC](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMC.h) to be used in a split simulation. The communication between the engines is handled by a singleton object of type [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMC.h) such that the user does not have to deal with special implementation details. Hence, pausing an engine, re-starting it or transferring tracks between them is done automatically and at the same time the user stack is kept up-to-date. Furthermore, the same user application derived from [TVirtualMCApplication](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMC.h) together with the same user stack derived from [TVirtualMCStack](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMC.h) can be used for both a single engine run or a simulation split among multiple ones.

Further general information on the VMC project can be found [here](https://root.cern.ch/vmc)

## How multiple engines are handled
To keep the overhead as small as possible, the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) object has to be requested by the user explicitly during construction of the user application by calling [TVirtualMCApplication::RequestManager()](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMCApplication.h#L55). That makes the manager available via the protected member [TVirtualMCApplication::fMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMCApplication.h#L155) and a pointer can also always be obtained via [TMCManager \*TMCManager::Instance()](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h#L55). In a single run, on the other hand, there is no such object and the VMC would directly interacts with the user's particle stack. In that case the scenario of how that VMC is treated and interacts with other objects is the same as it was in previous versions of the VMC package.

The most important interfaces of the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) to deal with multiple VMCs are:

* `void SetUserStack(TVirtualMCStack* userStack)`  
This notifies the manager about the user stack such that it will be kept up-to-date during the simulation. Not setting it makes the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) abort the execution.
* `void ForwardTrack(Int_t toBeDone, Int_t trackId, Int_t parentId, TParticle* userParticle)`  
The user is always the owner of all track objects (aka `TParticle`) being created. Hence, all engine calls to `TVirtualMCStack::PushTrack(...)` are first redirected to the user stack where a `TParticle` has to be created on the heap. After that, this method has to be invoked to forward the pointer of the created `TParticle` object. If a particle should be pushed to an engine other than the one currently running, the engine's ID has to be provided as the last argument.
* `void TransferTrack(Int_t targetEngineId)`  
For instance during `TVirtualMCApplication::Stepping()` the user might decide that the current track should be transferred to another engine, e.g., if a certain volume is entered. By specifying the ID or the pointer to the target engine the manager will take care of interrupting the track in the current engine, extracting the kinematics and geometry state and it will save these information for the target engine.
* `template <typename F> void Apply(F f)`  
This assumes `f` to implement the `()` operator and taking a [TVirtualMC](https://github.com/vmc-project/vmc/blob/master/source/include/TVirtualMC.h) pointer as an argument. `f` will be then called for all engines.
* `template <typename F> void Init(F f)`  
This works as `TMCManager::Apply` during the initialization of the engines. It can also be called without an argument such that no additional user routine is included.
* `void Run(Int_t nEvents)`  
A run involving all registered VMCs is steered for the specific number of events.
* `void ConnectEnginePointers(TVirtualMC *&mc)`  
This gives the possibility for a user to pass a pointer variable which will always be set to point to the currently running engine. Note, however, that there is a protected member `TVirtualMCApplication::fMC` which always points to the currently running engine and it can therefore be used within the application's code.
* `TVirtualMC *GetCurrentEngine()`  
This provides the user with a pointer to the currently running engine.

An example of how the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) is utilized in a multi-run can be found in E03c example of the [GEANT4_VMC repository](https://github.com/vmc-project/geant4_vmc/examples/E03/E03c). The `diff` of e.g **E03/E03a** and **E03/E03c** nicely highlights the small amount of extensions necessary to use the same application and stack for both a single and a multi-run.

### Implementation and workflow example
The general workflow of a multi-run is explained using some of the classes/implementations of the [E03c example](https://github.com/vmc-project/geant4_vmc/examples/E03/E03c) mentioned above.

**Implementation**

1. Implement your application and stack as you have done before. Request the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) in your constructor by calling `TVirtualMCApplication::RequestMCManager()`
{{< highlight cpp >}}
Ex03MCApplication::Ex03MCApplication(const char* name, const char* title, Bool_t isMulti, Bool_t splitSimulation)
{
        // some construction
        // user stack
        fStack = new Ex03MCStack(1000);
        if(isMulti) {
            RequestManager();
            fMCManager->SetUserStack(fStack);
        }
        // further construction
}
{{< /highlight >}}
1. At an appropriate stage (i.e. in `UserStack::PushTrack(...)`) you would call `TMCManager::ForwardTrack(...)` to forward the pointer to your newly constructed `TParticle` object.
{{< highlight cpp >}}
void Ex03MCStack::PushTrack(Int_t toBeDone, Int_t parent, ..., Int_t& ntr, ...)
{
        // TParticle construction yielding pointer "particle"
        // User defines the track ID
        ntr = GetNtrack() - 1;

        // Forward to the TMCManager in case of multi-run
        if(auto mgr = TMCManager::Instance()) {
            mgr->ForwardTrack(toBeDone, ntr, parent, particle);
        }
        // further implementations
}
{{< /highlight >}}
1. Whenever needed, check the presence of [`TMCManager`](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) to decide whether the code should work for single or multi-run, e.g. in the constructor of `Ex03DetectorConstruction` where its member `fMC` is setup to point to the currently running VMC in the multi-run scenario.
{{< highlight cpp >}}
Ex03DetecorConstruction::Ex03DetectorConstruction(...)
{
        // some construction
        if(auto mgr = TMCManager::Instance()) {
            mgr->ConnectEnginePointer(fMC);
        // ...
        }
        // maybe some more construction
}
{{< /highlight >}}
1. Transfer a track involving `TMCManager::TransferTrack(...)`. If the target VMC ID coincides with the one currently running, nothing will happen and the simulation would just continue.
{{< highlight cpp >}}
Ex03MCApplication::Stepping()
{
        // some implementation
        Int_t targetId = -1;
        if (fMC->GetId() == 0 && strcmp(fMC->GetCurrentVolName(), "ABSO") == 0) {
            targetId = 1;
        }
        else if (fMC->GetId() == 1 && strcmp(fMC->GetCurrentVolName(), "GAPX") == 0) {
            targetId = 0;
        }
        if (targetId > -1) {
            if (fVerbose.GetLevel() > 2) {
                Info("Stepping", "Transfer track");
            }
            fMCManager->TransferTrack(targetId);
        }   
        // further implementations
}
{{< /highlight >}}
To use the same stack in both scenarios (single and multi-run), one can always check whether the manager is present by checking if `TMCManager::Instance()` returns a valid pointer value.

**Usage**

1. Instantiate your application (here the user stack is created during construction of the application, see above)
{{< highlight cpp >}}
auto isMulti = true;
auto splitSimulation = true;
auto appl = new Ex03MCapplication("multiApplication", "multiApplication", isMulti, splitSimulation);
{{< /highlight >}}
1. Instantiate the engines you want to use (these are registered automatically to the manager). In the E03c example this is done in `TVirtualMCApplication::InitMC` but to clarify, it is written here explicitely,
{{< highlight cpp >}}
auto engine1 = new TGeant3(...);
auto engine2 = new TGeant4(...);
// Nothing further to be done
{{< /highlight >}}
1. Call `TMCManager::Init(...)` (if needed with your custom initialization procedure, see above; that can of course also be wrapped into another method of your application as it is done here),
{{< highlight cpp >}}
void Ex03MCApplication::InitMC(std::initializer_list<const char*> setupMacros)
{
      // some implementation
      fMCManager->Init([this](TVirtualMC* mc) {
        mc->SetRootGeometry();
        mc->SetMagField(fMagField);
        mc->Init();
        mc->BuildPhysics();
      });
      // further implementations
}
{{< /highlight >}}
The lambda argument will be called for each registered VMC separately. If necessary, it can be more specific. Instead of passing a lambda function, a function pointer could be given or also an object. The only requirement is that what is passed implements a `()` operator taking a VMC pointer as an argument.
1. Call `TMCManager::Run(...)` specifying the desired number of events to be simulated.
{{< highlight cpp >}}
void Ex03MCApplication::RunMC(Int_t nofEvents)
{
      // some implementation
      fMCManager->Run(nofEvents);
      // further implementations
}
{{< /highlight >}}

**Important comments**

The geometry is built once centrally via the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) calling

1. `TVirtualMCApplication::ConstructGeometry()`,
2. `TVirtualMCApplication::MisalignGeometry()`,
3. `TVirtualMCApplication::ConstructOpGeometry()`

and therefore, it is expected that these methods do not depend on any engine.

If multiple engines have been instantiated, never call `TVirtualMC::ProcessRun(...)` or other steering methods on the individual engine because that would bypass the [TMCManager](https://github.com/vmc-project/vmc/blob/master/source/include/TMCManager.h) and will lead to inconsistent behavior.
