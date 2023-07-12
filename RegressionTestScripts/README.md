This folder contains the files required for clustering and reconstruction. They have been ported over from nEXO\_MC. 

Clustering.py has been modified to read sniper (DetSimAlg) output as Clustering\_offline.py . 
Reconstruction.py has been modified to read the new Clustering\_offline.py output. 

To run Clustering execute: 

```bash
python Clustering_offline.py -c Baseline2017.card -i InputFileName.root -o OutputFileName.root
```

and similarly for Reconstruction:

```bash
python Reconstruction_offline.py -c Baseline2017\_offline.card -i Clustering\_offline-output.root -o OutputFileName.root -s seed
```
where seed is an int

To run MakeSimplePlots\_offline.py

```bash
python MakeSimplePlots_offline.py /path/to/inputFile.root /path/to/output_simplePlots.root /Event/Sim/SimEvent g4 
```
where ```/Event/Sim/SimEvent g4``` can be replaced by ```clusterTree cluster``` or ```reconTree recon``` depending on which type of input file you have accordingly. 


Status and TODO:

1. Port MakeSimplePlotsDecayChain.py nEXO_MC and test it with nexo-offline and move out of Old_ToBePorted/.
2. Port AddDecayChainInfo.py and test it with nexo-offline and move out of Old_ToBePorted/.


---------------------------------------------------------------------------------------

A (under development) sniper module/algorithm can be found under nexo-offline/Analysis/ReconstructionAlg/ and will contain the full sniperized reconstruction/clustering scripts and sniper module. 

Feel free to update this code

contact: soud.alkharusi@mail.mcgill.ca
