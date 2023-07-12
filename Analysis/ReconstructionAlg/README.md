The files found in here are under development to replace Clustering.py and Reconstruction.py from nEXO_MC. To be used as a new sniper module. 

The algorithm takes in data produced from RunDetSim.py (formerly RunSniper.py, nexo-offline Monte Carlo) and clusters the energy deposits in the TPC, applying basic detector response and energy resolution. 

Usage: 

In cmt/ execute:

```bash
cmt br cmt config
source setup.sh
cmt br cmt make
```

Then in share/ execute:
```bash
python RunReconstructionAlg.py --input /name/and/path/input.root --output /path/and/name/output.root
```




TO DO:

1. Implementation of Reconstruction.py and filling reconTree. 
2. Take in Cards (in YAML format) for all input parameters, currently these are hard-coded in. 

contact: soud.alkharusi@mail.mcgill.ca

