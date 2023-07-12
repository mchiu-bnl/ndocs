# Installing and running on the SLAC clusters

** Note: ** this page is mostly ported over from the nEXO wiki page here: https://ntpc.ucllnl.org/nEXO/index.php/Simulations_at_SLAC

## Installation

Installation area: `/nfs/slac/g/nexo/software/dev/`

## Installation on a Centos7 machine: Using nexo-offline on SLAC ===

### Using existing nexo-offline

Log onto a SLAC Centos7 machine:  ssh <username>@centos7.slac.stanford.edu

If you cannot make a directory, or don't have permissions to do so, you will need to execute /usr/bin/aklog after logging in.

1. Type `groups` at the command line and make sure `nexo` is listed
among them. If it is not, contact Heather about having you added to the nexo group.

2. In your `~/.bashrc` or `~/.bash_profile` file add the following lines:
`export NEXOTOP=/nfs/slac/g/nexo/software/dev/rh7-gcc48/nexo-offline-master` this will allow you to use the master branch of nexo-offline
`source $NEXOTOP/setup.sh`

3. In your home directory, add a folder called nexo-offline-work (or similar) and in there add `RunSniper.py` and whatever macro files you need and remember to execute `source ~/.bashrc` (or equivalent) or log off/back in and get another access token e.g. `/usr/bin/aklog`

4. Create or use an existing macro to run your simulation by executing `python ./RunSniper.py --run macroFile.mac --seed num --evtmax num --output /path/to/output.root`


If you require to modify source code (e.g. to add in macro commands, new geometries, new physics/outputs etc... ) then the best way is to set a new NEXOTOP with the modified code there and do everything else the same. 

For questions you can ask Soud or Heather, or post on the `\#slac` channel on the `nexosim` Slack. 

### Building your own nexo-offline against existing externals

(Disclaimer: This is just what worked for me (RT). Your mileage may vary. )

Create a "NEXOTOP" directory where your code sits and make necessary symlinks
```
$ mkdir -p $HOME/nexo/software
$ cd $HOME/nexo/software
$ ln -s /nfs/slac/g/nexo/software/dev/rh7-gcc48/ExternalLibs
$ ln -s /nfs/slac/g/exo-userdata/users/zpli/nEXO/NEXOTOP/sniper_install    # Borrowing Zepeng's sniper
$ git clone git@github.com:nEXO-collaboration/nexo-offline.git
```

Create a setup file at `$HOME/nexo/software/setup.sh` that looks like the one below:
```
export NEXOTOP=$HOME/nexo/software

# External libraries
export EXTLIB=${NEXOTOP}/ExternalLibs

# Set up
source ${EXTLIB}/Cmake/3.11.1/bashrc
source ${EXTLIB}/Python/2.7.15/bashrc     # cmake still finds python 3.6.
source ${EXTLIB}/Geant4/10.04.p01/bashrc
source ${EXTLIB}/ROOT/6.12.06/bashrc
source ${EXTLIB}/Xercesc/3.1.2/bashrc
source ${EXTLIB}/gsl/1.16/bashrc
source ${EXTLIB}/vgm/4.4/bashrc
source ${EXTLIB}/Boost/1.67.0/bashrc      # CMake complains: "Could NOT find Boost". 
#source ${EXTLIB}/NEST/2.1.0/bashrc       # During compile, gcc complains: cannot find NESTProc.hh
#source ${EXTLIB}/fftw/3.3.8/bashrc       # No such directory

# Tell gcc where pyconfig.h is.
export NEXO_EXTLIB_Python_HOME=${EXTLIB}/Python/2.7.15
export PATH=${NEXO_EXTLIB_Python_HOME}/bin:${PATH}
export LD_LIBRARY_PATH=${NEXO_EXTLIB_Python_HOME}/lib:${LD_LIBRARY_PATH}
export CPATH=${NEXO_EXTLIB_Python_HOME}/include/python2.7:${CPATH}
export PKG_CONFIG_PATH=${NEXO_EXTLIB_Python_HOME}/lib/pkgconfig:${PKG_CONFIG_PATH}
export PYTHONPATH=${NEXO_EXTLIB_Python_HOME}/lib/python2.7/lib-dynload:${PYTHONPATH}

# Set up Sniper
source ${NEXOTOP}/sniper_install/setup.sh

# Stolen from Zepeng
export ZP_EXTLIB=/u/xo/zpli/data/nEXO/NEXOTOP/ExternalLibs

# Set up NEST
export NEST_DIR=${ZP_EXTLIB}/Build/NEST/install
export LD_LIBRARY_PATH=$NEST_DIR/lib:$LD_LIBRARY_PATH
export PATH=$NEST_DIR/bin:$PATH

#Set up FFTW (From Zepeng)
export FFTW_DIR=${ZP_EXTLIB}/Build/fftw-3.3.8/
export LD_LIBRARY_PATH=$FFTW_DIR/lib:$LD_LIBRARY_PATH
export PATH=$FFTW_DIR/bin:$PATH
```

Finally, cmake and build
```
$ source setup.sh
$ mkdir nexo-build
$ cd nexo-build
$ cmake -DPYTHON_INCLUDE_DIR=/nfs/slac/g/nexo/software/dev/rh7-gcc48/ExternalLibs/Python/2.7.15/include \
-DPYTHON_LIBRARY=/nfs/slac/g/nexo/software/dev/rh7-gcc48/ExternalLibs/Python/2.7.15/lib/libpython2.7.so \
-DPYTHON_EXECUTABLE=/nfs/slac/g/nexo/software/dev/rh7-gcc48/ExternalLibs/Python/2.7.15/bin/python \
../nexo-offline
$ make
$ source setup.sh
```

To run, create a job script called `run.sh` such as the one below:
```
#!/bin/bash
isotope=$1
nevts=$1

source /u/xo/hmtsang/nexo/software/setup.sh
source /u/xo/hmtsang/nexo/software/nexo-build/setup.sh
python RunFullChain.py --seed 1 --evtmax ${nevts} --run examples/ExternalGammas-${isotope}.mac --output output/ExternalGammas-${isotope}-${nevts}.root \
> output/ExternalGammas-${isotope}-${nevts}.out 2>&1
```
Then submit the job with this command:
```
bsub -R centos7 -q long -o ExternalGammas.out -W 6:00 ./run.sh Tl208 1000
```

