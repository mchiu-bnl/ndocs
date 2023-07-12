# SensitivityRecon data structure

The output of the SensivitiyRecon reconstruction code is a ROOT file containing a TTree called `TREENAME` 
(located in the TDirectory path `Event/Path/TREENAME`)

The data members described below are defined in the various header files in [DataModel/Sensitivity/Event/](../DataModel/Sensitivity/Event/)

Primary Event Variables
------
* **fNTE**:  Total number of thermal electrons generated in the event (should mirror **fNTE** in the `SimEvent` tree)
* **fEnergy**: Total energy deposited in LXe in the event (should mirror **fTotalEventEnergy** in the `SimEvent` tree)

Other header
------

