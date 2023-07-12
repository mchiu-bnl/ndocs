# Installation
## Prerequisites
* An github account with permission to clone code (contact brodsky3@llnl.gov with your github account name)
* cmake >=v2.8
* SNiPER 2.1 (which has its own dependencies, e.g. XERCES-C and Boost)
* VGM
* ROOT
* GEANT4
* python3

For installing these dependancies, it is highly recommended that you use the spack package manager. 
See: https://github.com/nEXO-collaboration/nexo-spack-env

These prerequisites are already installed on [LLNL](llnl.md) and in the [Docker image](docker.md), so if you have access to these clusters it is highly recommended you work there instead.

## Get the source code, and set up build directory
```
git clone git@github.com:nEXO-collaboration/nexo-offline.git
mkdir build
cd build/
```
## Use Cmake to configure the build
### If not using spack
Set up GEANT4 and ROOT environment variables as normal 

```
source /path/to/root-install/bin/thisroot.sh
source /path/to/geant4-install/bin/geant4.sh
```
From the nexo-offline build directory:
```
cmake -DSNIPER_ROOT_DIR=/path/to/sniper-install/ -DVGM_DIR=/path/to/vgm/lib64/VGM-4.x.x -DNEST_DIR=/path ../source/
```
(Replace the VGM-4.x.x with the version number installed)

You can instead use
```
ccmake ../source 
```
to see a list of cmake variables that may need to be set. ccmake can be especially helpful for switching between Release and Debug builds.

### If using spack
```
source /path/to/spack/share/spack/setup-env.sh
spacktivate /path/to/nexo-spack/env
```

```
cmake -DCMAKE_CXX_STANDARD=17 ../nexo-offline/
```

## Build nexo-offline
From the nexo-offline build directory, run
```
make -j4
```
(or replace -j4 with your preferred number of threads to use)


## After installation

From the build directory:
```asm
source setup.sh
```
To set relevant environment variables for loading nexo-offline libraries. You can include this setup script in your
profile if you'd like to run it every login.

From the nexo-offline (source) directory:
```
cd data
python3 download.py
```
to acquire important data files

``export NEXOTOP= /path/to/nexo-offline/..``

Some scripts use the `NEXOTOP` environment variable to find important data files. 
This should point to the directory *above* a folder named `nexo-offline`, which in turn contains a `data` folder.
