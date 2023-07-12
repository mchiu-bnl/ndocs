# nexo-offline
This repository includes several parts:
* Data Model
* Simulation
  * Detector simulation
  * Digitization simulation
* Reconstruction

# Ancillary Files
## Charge digitization sim
Five of the required data files for the ChargeSim are [available on Google Drive](https://drive.google.com/drive/folders/1pGs95XpBhZtxLNasiFAFIxN3MS2O9aJL?usp=sharing) and can be be downloaded and verified by running data/download.py:
* The noise library (noise_lib_1_2_us_100e.root)
* The weight potential data file (singlePadWP3mm.root)
* An E-field bug fix (ckpt_0816_BugFix_uniform_E_pos.t7)
* The most recent light-map (20200528_lightmap_0.50_0.50.root)
* The cryo-ASIC response function (cryo_asic_model.root)

There are several different data files, corresponding to different 
assumptions on the charge readout geometry and the noise. 
As of May 2020, nEXO plans to use 6mm strips, with 200e expected noise.

If you're running on the LLNL cluster, all of these can be found in the shared directory `/usr/gapps/nexo/nexo-offline/data/`.

The noise library included in the google drive (noise_lib_1_2_us_100e.root) is the standard noise library that most users should be using. If you need specialized noise traces with different paramters for your work, such as RMS, shaping time etc, this can be done with noise_library_generator.py which is found in the data directory. By adjusting the user paramters inside this script and running it, the ASIC spectrum saved in outnoise_tps.csv will be used to generate a new noise library with those paramters.

You will also need two data files which map channels to physical locations:
* A tile map (e.g. tilesMap_6mm.txt), which describes where the tiles are located in the detector.
* A channel map (e.g. localChannelsMap_6mm.txt), which describes where different channels are within a tile.

These are stored in this repository, in the directory `data/`. They are also
stored on the LLNL cluster at the path given above.


# Installing/Running/Developing the software
See [Doc](Doc/README.md).

# About this repository
Please join the offline developers team.
