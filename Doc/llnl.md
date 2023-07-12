# LLNL

All dependencies for nexo-offline are available by configuring your environment with:
```
source /p/lustre1/nexouser/spack_setup.sh
```
See https://github.com/nEXO-collaboration/nexo-spack-env for the versions of the dependencies.

Sometimes this step can run very slowly or not at all, due to weird interactions with the file system. If that's the case, 
```
source /p/lustre1/nexouser/spack_setup_nexo_lite.sh
```
is an alternative that should work in most cases.

If desired, either of these commands can be added to your `.bashrc` to load them on startup.

# Install your own copy of nexo-offline

```
source /p/lustre1/nexouser/spack_setup.sh
mkdir nexo-offline
git clone git@github.com:nEXO-collaboration/nexo-offline.git 
mkdir build
cd build/
 
cmake -DCMAKE_CXX_STANDARD=17 -DCMAKE_BUILD_TYPE=RelWithDebInfo ../nexo-offline/
 
cmake --build . --target all -- -k
cd -
source build/setup.sh
```

# Quick Test Running nexo-offline

To test that everything was built correctly, we'll need to run an example script. But first, we'll need to install the necessary files. This can be done directly from the google drive, or by navigating to `nexo-offline/data` and running

```
python3 download.py --no-check-certificate
```
If you get an error at this step, double check that you've installed your dependancies with spack as described above.

We now need to setup the enviormental variables used in nEXO-offline. Navigate back to your `build` directory and run the two commands below.

```
export NEXOTOP=/path/to/nexo-offline/..
source setup.sh
```
Where `/path/to/nexo-offline/..` is the absolute file path to your local installation of nexo-offline.

Now we can actually run the test script. This is done using the command below.

```
cd Cards
python3 RunDetSim_new.py --digioutput test.root --evtmax 1000 --seed 1 --run examples/TPCVesselU.mac > test.out 2> test.err
```

This will save the output of the simulation to `build/Cards/test.root`. You'll want to open this file (uproot or VS's RootViewer are recommended) and look at the `Event/Sim/SimEvent/fEnergyDeposit` data. If the code is running correctly, the histogram should ressemble a 1/f distribution, with a peak at ~0.02


# Running your own custom simulations

Once you have the software installed, you can run your own simulations and do some science! 
See the docs here: [Running your own simulations](running_detsim_simulations.md)


# References
[https://ntpc.ucllnl.org/nEXO/index.php/Installing_nEXO_MC_at_LLNL](https://ntpc.ucllnl.org/nEXO/index.php/Installing_nEXO_MC_at_LLNL)
