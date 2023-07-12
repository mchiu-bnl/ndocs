# Using Singularity
Docker images can be run directly using Singularity.  Most computing centers have Singularity availble for general use.

## Singularity At SLAC

Using the SLAC Public Centos7 machines, the latest release of Singularity 3.5 is installed, and should be available in your path when you log in via `<userName>@centos7.slac.stanford.edu`

NOTE: make sure you are a member of the `nexo` group. You can check by typing the command:
* `groups`
at the command prompt. If you do not see `nexo` in the output, contact Heather about adding you to the group.

#### A note about ancillary data

Additional data files are often required for recent (March 2020) runs of nexo-offline.  These can be found in `/nfs/slac/g/nexo/software/prod/singularity/nexo-offline/data`

### To Run the nexo-base Docker image at SLAC and compile your own copy of nexo-offline
The nexo-base docker image includes all necessary external libraries as well as SNiPER.  The image does not include nexo-offline (or nEXO_MC).
To use this image, you must supply your own local copy of nexo-offline.  From a `bash` shell, do the following and note your copies of `nexo-offline` and `nexo-offline-build` should reside in your current `$PWD`:

* `git clone https://github.com/nEXO-collaboration/nexo-offline.git`
* `mkdir nexo-offline-build` 
* `singularity shell --home $PWD /nfs/slac/g/nexo/software/prod/singularity/nexo-base_v4r1p0.sif`
* `cd /opt/nexo/software`
* `source bashrc.sh`
* `source sniper-install/setup.sh`
* `cd $HOME/nexo-offline-build`
* `cmake -DSNIPER_ROOT_DIR=/opt/nexo/software/sniper-install -DCMAKE_CXX_FLAGS="-std=c++11" ../nexo-offline`
* `make`


### Quick Test 

```
source /opt/nexo/software/bashrc.sh
source /opt/nexo/software/sniper-install/setup.sh
source $HOME/nexo-offline-build/setup.sh
cd $HOME/nexo-offline-build/Cards
python ./RunDetSim.py --evtmax 100 --seed 1 --run ./examples/TPCVesselU.mac --output TPCVesselU.root > TPCVesselU.out 2>TPCVesselU.err
```

### Running Singularity at SLAC after you have already built your copy of nexo-offline

After you have successfully built your copy of `nexo-offline`, you can restart the Singularity image and run the code, without any need to rebuild nexo-offline (unless you have made code changes).` $PWD` should be the directory that includes your `nexo-offline-build`

```
singularity shell --home $PWD /nfs/slac/g/nexo/software/prod/singularity/nexo-base_v4r1p0.sif
cd /opt/nexo/software
source bashrc.sh
source sniper-install/setup.sh
cd $HOME/nexo-offline-build
source setup.sh
cd Cards
python ./RunDetSim.py --evtmax 100 --seed 1 --run ./examples/TPCVesselU.mac --output TPCVesselU.root > TPCVesselU.out 2>TPCVesselU.err
```

### Admin Functions

#### Installing docker images at SLAC

Here are the steps to upload an existing docker image and store it as a SIF at SLAC

* cd /nfs/slac/g/nexo/software/prod/singularity
* source setup-addimages.sh
* singluarity pull docker://docker_repo/container_name:tag_name    i.e. singularity pull docker://heather999/nexo-base:v4r2p0

When completed a new *.sif file will be added to the current directory.
