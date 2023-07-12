# `nexo-offline` software

## Getting started

### Installing/running the code
  * On your own machine 
    + [From scratch](installation.md)
    + [Using the docker image](docker.md) (may be outdated)
    
  * Running at LLNL
    + [Using your own installation](llnl.md)
  * Running at SLAC
    + [Using singularity](singularity.md) (recommended)
    + [Using your own installation](slac_installation.md)

### Running your own simulations

  * Introduction to nexo-offline (under construction)
  * Working with Geant4 macro files
    + [Tutorial](running_detsim_simulations.md)
    + [List of Geant4 macro commands](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/BackupVersions/V10.5-2.0/html/Control/AllResources/Control/UIcommands/_.html)
    + [List of custom nexo-offline macro commands](macro_commands.md)
  * Visualizing the Geant4 simulation geometry
    + [Vis notes](https://github.com/nEXO-collaboration/nexo-offline/wiki/Simulation-Visualizations)
  * Working with the SNiPER framework
    + [nEXO wiki page on SNiPER integration](https://ntpc.ucllnl.org/nEXO/index.php/Sniper_Integration)
    + [Raymond's SNiPER tutorial from 2017](https://ntpc.ucllnl.org/nEXO/images/3/34/171212-sniper-sim.pdf)
  * Using the card system
    + [Docs/Tutorial](../Cards/README.md)

### Analyzing the simulation data

  * Data structure descriptions
    + [SimEvent data (DetSim output)](SimEventDescription.md)
    + [ElecEvent data (Digitization output)](ElecEventDescription.md)
    + [Sensitivity (Reconstruction output)](SensitivityReconDescription.md)
  * Analyzing data in python using `uproot`
    + [Short tutorial](https://github.com/nEXO-collaboration/nexo-offline/wiki/Jupyter-notebook-and-uproot)

### Working with GitHub
  + [HOWTO](CommittingChangesHowto.md)


## For developers

* [SNiPER](sniper.md)
+ [Docker image](docker.md)
* [Code style](CodeStyle.md)
* [Using git](UsingGit.md)
* [Code Review](DoingCodeReview.md)
