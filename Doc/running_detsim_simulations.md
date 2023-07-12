# Running your own simulations

This doc contains some tips for customizing the Geant4-based part of the 
simulations, called `DetSim` in the code. This is distinct from the 
simulation of the actual signals, which is done in the `Digitization` 
section of the code. 

The Geant4-based `DetSim` simulates particle transport as well as various 
interactions (i.e. radioactive decays, neutron capture, etc.). As with any 
Geant4 application, the simulation inputs and outputs can be customized 
using macro files, which by convention end with `.mac`.

Note that Geant4 has its own documentation here:
* Geant4 website: [https://geant4.web.cern.ch/](https://geant4.web.cern.ch/)
* [Geant4 User Documentation](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/InstallationGuide/BackupVersions/V10.5-2.0/html/index.html)

# Customizing a macro file

Macro files contain a list of commands that the simulation will execute in order 
to produce the desired results. A full list of available commands are documented 
in one of two places:
* [List of commands in Geant4](http://geant4-userdoc.web.cern.ch/geant4-userdoc/UsersGuides/ForApplicationDeveloper/BackupVersions/V10.5-2.0/html/Control/AllResources/Control/UIcommands/_.html) - 
these are commands that are available in any Geant4 application.
* [List of commands specific to `nexo-offline`](macro_commands.md) - A (hopefully up-to-date) list of commands specific to the nEXO simulation package.


## Tutorial: Anatomy of a macro file
We can start with the file [`TPCVesselU.mac`](../Cards/examples/TPCVesselU.mac), 
which is located in `Cards/examples/` from the `nexo-offline` source diretory. 
I'll walk through the various pieces of this file. (The comments in the file 
also more or less explain what's going on.)

We start by setting up
various verbosity flags to control what will be printed out as the simulation runs. 
By default, these are set to 0, so we don't actually need to include commands
which set the verbosity to 0. However, they're left here for reference. 
Next, we perform an overlap check on the geometry, to make sure there aren't any 
conflicts. We then execute a *second* macro file, called 
[`Baseline2019.mac`](../Cards/yamls/Baseline2019.mac), which sets up the correct
geometry: 
```
# Turn on/off verbosity in various places
/run/verbose 0
/tracking/verbose 0
/control/verbose 1

# Check for overlaps in the geometry
/nEXO/det/checkOverlap true


# Use Baseline 2019 geometry
/control/execute ./yamls/Baseline2019.mac
```


The next part contains some commands to set up the ionization and scintillation
physics processes. There are two particles that represent the signals that nEXO
actually detects:
* **Optical photons** (abbreviated OP) - these are the scintillation photons that
will be created when energy is deposited in the liquid xenon
* **Thermal electrons** (abbreviated TE) - this is the charge released via ionization
when energy is deposited in the liquid xenon

Note that some of the commands below are commented out (comments in macro files
begin with  a `#`). This means that they won't actually
be executed by this macro --  the macro will use whatever the default options are. 
They are only written out for completeness:

```
# If you want to record thermal electrons and
# scintillation photons, need to turn NEST on
# using the following switch:
/EXOPhysicsList/enableLight true

# If you want to turn on/off optical photons or 
# thermal electrons, uncomment the appropriate
# line below:

#/analysis/setSaveOP true
#/analysis/setSaveTE true

# If you want to do photon tracking with Geant4,
# uncomment the line below.
# Note that unless this is turned on, fNOP will
# be set to 0 for all events even if NEST is on.

#/analysis/setPropagateOP 1
```

Next we initialize the run. This line is required in every macro.
```
# This is required for all Geant4 simulations.
/run/initialize
```

And finally, we set up the actual particle(s) that we want to simulate.
```
# Use Geant4's GPS (general particle source)
/generator/setGenerator gps 

# Create an ion with Z=83, A=214 (corrseponding to Bi214)
/gps/particle ion 
/gps/energy 0 keV 
/gps/ion 83 214 
/grdm/nucleusLimits 214 214 83 83

# Establish the geometry for the source. Here we 
# create a cylinder with the dimensions of the 
# nEXO TPC, but further require that events are
# confined inside the TPCVessel part of the 
# geometry (in case our dimensions aren't exact).
/gps/pos/type Volume
/gps/pos/shape Cylinder
/gps/pos/centre 0 0 -1022 mm
/gps/pos/radius 642.5 mm
/gps/pos/halfz 738.5 mm
/gps/pos/confine /nEXO/TPCVessel
```
This code creates a source of Bi214 nuclei at rest 
(the line `gps/energy 0 keV` refers to the particle's kinetic energy),
which will undergo radioactive decay. The radioactive decay will release
gamma rays, which will then be tracked throughout the geometry by Geant4.
We also establish the position of the source: the `/gps/pos` commands
create a cylinder which is confined to the TPCVessel, meaning the Bi214 
nuceli will be randomly generated anywhere inside the TPCVessel volume. 
Note that the TPCVessel refers to the copper vessel which contains the xenon,
so the Bi214 nuclei will only be generated in this thin shell.

# Checking the output by turning tracking verbosity on

An easy way to check if the simulation is doing what you want is to run 
one or two events with the tracking verbosity turned on, and look at the
output. We can do this in the example [`TPCVesselU.mac`](../Cards/examples/TPCVesselU.mac)
by changing the line 
```
/tracking/verbose 0
```
to
```
/tracking/verbose 1
```
**IMPORTANT NOTE:** Remember to set this back to zero before you run 
more than a handful of events. It will print out a LOT of text for each 
event, which can slow things down and create enormous log files.

and then running it on the command line

```
python ./RunDetSim.py --evtmax 1 --seed 1 --run ./examples/TPCVesselU.mac --output TPCVesselU.root
```

Now, you should see a long printout of various particle tracks, which look something like 
what I show below. Note that in this example, the output will be much longer than
what is reproduced here -- I've truncated it just for clarity.


```
*********************************************************************************************************
* G4Track Information:   Particle = Bi214,   Track ID = 1,   Parent ID = 0
*********************************************************************************************************

Step#    X(mm)    Y(mm)    Z(mm) KinE(MeV)  dE(MeV) StepLeng TrackLeng  NextVolume ProcName
    0 -592.760  219.314 -374.650     0.000    0.000    0.000     0.000 /nEXO/TPCVessel initStep
    1 -592.760  219.314 -374.650     0.000    0.000    0.000     0.000 /nEXO/TPCVessel S1

*********************************************************************************************************
* G4Track Information:   Particle = e-,   Track ID = 4,   Parent ID = 1
*********************************************************************************************************

Step#    X(mm)    Y(mm)    Z(mm) KinE(MeV)  dE(MeV) StepLeng TrackLeng  NextVolume ProcName
    0 -592.760  219.314 -374.650     0.082    0.000    0.000     0.000 /nEXO/TPCVessel initStep
    1 -592.762  219.312 -374.650     0.000    0.082    0.018     0.018 /nEXO/TPCVessel eIoni
    2 -592.762  219.312 -374.650     0.000    0.000    0.000     0.018 /nEXO/TPCVessel S1

*********************************************************************************************************
* G4Track Information:   Particle = Po214[1742.980],   Track ID = 2,   Parent ID = 1
*********************************************************************************************************

Step#    X(mm)    Y(mm)    Z(mm) KinE(MeV)  dE(MeV) StepLeng TrackLeng  NextVolume ProcName
    0 -592.760  219.314 -374.650     0.000    0.000    0.000     0.000 /nEXO/TPCVessel initStep
    1 -592.760  219.314 -374.650     0.000    0.000    0.000     0.000 /nEXO/TPCVessel RadioactiveDecay

*********************************************************************************************************
* G4Track Information:   Particle = gamma,   Track ID = 6,   Parent ID = 2
*********************************************************************************************************

Step#    X(mm)    Y(mm)    Z(mm) KinE(MeV)  dE(MeV) StepLeng TrackLeng  NextVolume ProcName
    0 -592.760  219.314 -374.650     1.134    0.000    0.000     0.000 /nEXO/TPCVessel initStep
    1 -591.977  221.213 -372.418     1.134    0.000    3.033     3.033 /nEXO/TPCExternals/HFE Transportation
    2 -579.298  251.941 -336.290     0.718    0.000   49.093    52.126 /nEXO/TPCExternals/HFE compt
    3 -523.963  276.431 -310.136     0.500    0.000   65.922   118.049 /nEXO/TPCExternals/HFE compt
    4 -494.188  288.933 -225.239     0.403    0.000   90.831   208.880 /nEXO/TPCExternals/HFE compt
    5 -517.555  327.378 -154.114     0.183    0.001   84.159   293.039 /nEXO/TPCExternals/HFE compt
    6 -542.928  304.090 -166.439     0.119    0.000   36.580   329.619 /nEXO/TPCExternals/HFE compt
    7 -550.058  326.870 -153.497     0.117    0.000   27.153   356.771 /nEXO/TPCExternals/HFE compt
    8 -616.133  388.824  -92.161     0.086    0.000  109.391   466.162 /nEXO/TPCExternals/HFE compt
    9 -612.478  402.334 -158.383     0.067    0.000   67.685   533.847 /nEXO/TPCExternals/HFE compt
   10 -613.634  394.886 -153.395     0.066    0.000    9.038   542.885 /nEXO/TPCExternals/HFE compt
   11 -606.052  359.106 -145.393     0.065    0.001   37.440   580.325 /nEXO/TPCExternals/HFE compt
   12 -602.210  355.719 -139.768     0.058    0.000    7.607   587.932 /nEXO/TPCExternals/HFE compt
   13 -609.938  353.472 -135.615     0.055    0.000    9.056   596.989 /nEXO/TPCExternals/HFE compt
   14 -624.908  373.479 -121.192     0.053    0.001   28.852   625.840 /nEXO/TPCExternals/HFE compt
   15 -632.076  377.402  -64.085     0.046    0.000   57.688   683.529 /nEXO/TPCExternals/HFE compt
   16 -634.104  377.702  -66.193     0.043    0.000    2.940   686.469 /nEXO/TPCExternals/HFE compt
   17 -660.947  392.925  -57.415     0.000    0.001   32.084   718.553 /nEXO/TPCExternals/HFE phot
.
.
.
<And it goes on like this for a while>
```

You can see that the simulation is indeed doing what we expect: a Bi214 atom is generated, and beta decays to
the 1742.980 keV excited state of Po214, which then de-excites by emitting gamma rays, and so on.



# Generating custom particles

You can choose which primary particles you'd like to simulate in one
of two ways:
* Using Geant4's General Particle Source (GPS), which allows one to
run a variety of particles, like electrons, muons, or specific radioactive atoms.
[Some helpful documentation on GPS can be found here](https://www.fe.infn.it/u/paterno/Geant4_tutorial/slides_further/GPS/GPS_manual.pdf)
* Using one of nEXO's custom built-in event generators. We have a list of available generators
located on [nEXO macro commands documentation page](macro_commands.md)

If, for example, I wanted to generate a point source of 1 MeV electrons in the 
center of the TPC, I could replace the following code (in the `TPCVesselU.mac` example):
```
# Create an ion with Z=83, A=214 (corrseponding to Bi214)
/gps/particle ion 
/gps/energy 0 keV 
/gps/ion 83 214 
/grdm/nucleusLimits 214 214 83 83

# Establish the geometry for the source. Here we 
# create a cylinder with the dimensions of the 
# nEXO TPC, but further require that events are
# confined inside the TPCVessel part of the 
# geometry (in case our dimensions aren't exact).
/gps/pos/type Volume
/gps/pos/shape Cylinder
/gps/pos/centre 0 0 -1022 mm
/gps/pos/radius 642.5 mm
/gps/pos/halfz 738.5 mm
/gps/pos/confine /nEXO/TPCVessel
```
with
```
/gps/particle e-
/gps/ang/type iso   # Note that this command is unneccessary, since it's "iso" by default
/gps/ene/type Mono  # Note that this command is unneccessary, since it's "Mono" by default
/gps/energy 1 MeV

/gps/pos/type Point
/gps/position 0 0 -1022 mm
```
where some of the commands above aren't strictly necessary in this particular example, 
but are included here for completeness.

**Note** that I've removed the `/gps/pos/confine`
command, because the electrons are a point source and we therefore don't need to 
confine the source to a particular volume.
