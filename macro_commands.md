# List of nEXO-specific macro commands

Below we document the various macro commands that are specific to `nexo-offline`. 

Note that our simulations also make use of the 
[much larger set of commands native to Geant4](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/BackupVersions/V10.5-2.0/html/Control/AllResources/Control/UIcommands/_.html); 
if there is a command that you need, and don't see it below,
try looking there. 




## EXOPhysicsList

| Command | Description | Allowed Values |
| ------- | ----------- | -------------- |
| /EXOPhysicsList/defaultCutValue | Minimum length to track particle in simulation* | `<float>` (default 1mm) |
| /EXOPhysicsList/cutValueInnerRegion | Miniumum length to track particle inside the outer cryostat* | `<float>` (default 100nm) |
| /EXOPhysicsList/cutValueInsideTPC | Minimum length to track particles in TPC* | `<float>` (default 100nm) |
| /EXOPhysicsList/enableLight | Toggle NEST and Cherenkov light production on/off | `<bool>` (default false) |
| /EXOPhysicsList/NESTVersion | Call a specific version of NEST. Version 1 is probably not supported anymore. | 1 or 2 (default 2) |
| /EXOPhysicsList/NESTYield | Track this fraction of NEST scintillation photons. Non-tracked photons are still recorded in the NESTHits output | 0 or 1 (default 1, enabled) |
| /EXOPhysicsList/NESTDetailed | Turn on NEST scintillation pulse shapes | `<bool>` (default true) |

\* Bremsstrahlung gammas below this length will not be tracked 



## Analysis

| Command | Description | Allowed Values |
| ------- | ----------- | -------------- |
| /analysis/printLogicVolumeNames | Print logical volume names to stdout | no parameter |
| /analysis/printVolumes | Print volume names to stdout | `<int>` (always on, input has no effect) |
| /analysis/printTotalTranslation | Print the location of a specific logical volume | `<volume name>` |
| /analysis/setPropagateOP | Turn on/off optical photons | 0: no </br> 1: yes </br> 2: only in water <br/> 3: propagate only 1\% in water  |
| /analysis/setPropagateTE | Turn on/off thermal electrons | `<int>` (default 0) |
| /analysis/setSaveOP | Turn on/off saving optical photons | `<int>` (LEGACY, currently does nothing) |
| /analysis/setSaveTE | Turn on/off saving thermal electrons | `<int>` (LEGACY, currently does nothing) |
| /analysis/setSaveOnlyEventsWithDeposits | only save events where energy is deposited in LXe | `<bool>` (default true) |
| /analysis/setSaveOnlyEventsWithXe137 | only save events where Xe137 is created | `<bool>` (default false) |
| /analysis/setSaveOnlyEventsWithNeutrons | only save events where neutrons are created | `<bool>` (default false) |
| /analysis/setSensitiveSteel | Set the steel water tank to record incident optical photons | `<bool>` (default false) |
| /analysis/setDayaBayPMT | Use Daya Bay PMT model for water tank analysis | `<bool>` (default true) |
| /analysis/setCosmogenics | significantly speeds up cosmogenics simulations by killing off low energy EM tracking |  `<bool>` (default false) |



## Detector

| Command | Description | Allowed Values |
| ------- | ----------- | -------------- |
| /nEXO/det/checkOverlap | check for overlaps in detector | `<bool>` |
| /nEXO/det/stepMax | Set a maximum step size for particle tracking | `<float>` (default is none) |
| /nEXO/det/UseStanfordTPCGeometry | Switch to using the geometry for the Stanford TPC | `<bool>` (default false) |




## Generator

| Command | Description | Allowed Values |
| ------- | ----------- | -------------- |
| /generator/setGenerator | Choose a particle generator | gps <br/> GeneralParticleSource <br/> gun <br/> bb0n <br/> bb2n <br/> nCaptureXe136 <br/> nCaptureCu <br/> B8NuElectronRecoil <br/> IBD <br/> CosmicMuon <br/> Neutron <br/> BiPoFieldRings <br/> BiPoSkin |
| /generator/setnCaptureXeSimMethod | Enter the LXe component to define n-capture decays | InternalConversions <br/> RandomGammas (default) <br/> ImbalanacedCascade |
| /generator/setXeComponent | Enter the LXe component to confine n-capture decays | ActiveLXe (default) <br/> InactiveLXe|
| /generator/setCuIsotope | Enter the Cu isotope for n-Capture | 63 (default) <br/> 65 |
| /generator/setCuComponent | Enter the copper component to confine n-capture decays | TPC (default) <br/>  APDPlatter <br/> FieldRing <br/> InnerCryo <br/>  OuterCryo |
| /generator/setBb2nCutOffMin | Enter the fraction (wrt the Q-value) of the minimum value for the sum of the electron energies | `<float>` (default 0.) |
| /generator/setBb2nCutOffMax | Enter the fraction (wrt the Q-value) of the maximum value for the sum of the electron energies | `<float>` (default 1.) |
| /generator/inCenter | If used, generators that normally pick a random location in the LXe instead always choose the center of the LXe. |`<bool>` (default false) |
| /generator/setSNenergySpectrum | Enter the supernova energy spectrum you'd like to sample from. | gkvm (default) <br/> livermore |
| /generator/setIBDenergy | Enter the energy you want for your IBD event. | `<float>` <br/> 0: (default) follows the SN spectrum  |
| /generator/setIBDThetaDirection | Enter the direction you'd like the supernova to come from | `<float>` (units of degrees) |
| /generator/setMuonThetaDirection | Enter the direction you'd like the muon to come from (theta only - in radians) | `<float>` (default 0.) <br/> will sample from distribution |
| /generator/setMuonEnergy | Enter the energy you want for your muon in GeV | `<int>` (default 0) <br/> will sample from distribution |
| /generator/setMuonCharge | Enter the charge you want for your muon | -1 or 1 (default 0, will use 1.25:1 ratio) | 
| /generator/setMuonTarget | Toggle muon generation | 0: no target <br/> 1: generate muons passing through a cylinder on cryopit floor radius = 655 cm, height = 14 m, corresponding to the largest water tank (default) <br/> 2: generate only muons passing through outer cryostat only |
| /generator/GeantinoTest | If used, only geantinos will be generated | `<bool>` (default false) |


## Water Shielding Tank

| Command | Description | Allowed Values |
| ------- | ----------- | -------------- |
| /nEXO/TPCExternals/WaterShieldTank/PMTConfigurationOption | PMT configuration in outer detector | `<int>` (default 0, no PMTs) | 
| /nEXO/TPCExternals/WaterShieldTank/TotalNumberOfPMTs | Number of PMTs in water tank | `<int>` |
| /nEXO/TPCExternals/WaterShieldTank/PMTUnitStandoff | | |
| /nEXO/TPCExternals/WaterShieldTank/setDayaBayPMT | | |


## Geometry

The nEXO gemoetry is parameterized using macro commands: see [Baseline2019.mac](../Cards/yamls/Baseline2019.mac) for a pretty comprehensive list. However, in most use cases,
you shouldn't need to make any modifications. Just call one of the preset geometries using the `/control/execute` command, 
i.e. by adding something like
```
/control/execute ./yamls/Baseline2019.mac
```
to your macro.

If you're running the StanfordTPC geometry, add the following switch to your macro file:
```
/nEXO/det/UseStanfordTPCGeometry true
```
Note that the dimensions here are hard-coded -- modifications will require modifying the source files:
* [StanfordLabConstructor.cc](../Simulation/DetSim/nEXOSim/src/StanfordLabConstructor.cc)
* [StanfordLabCryostatConstructor.cc](../Simulation/DetSim/nEXOSim/src/StanfordLabCryostatConstructor.cc)
* [StanfordLabTPCVesselConstructor.cc](../Simulation/DetSim/nEXOSim/src/StanfordLabTPCVesselConstructor.cc)
