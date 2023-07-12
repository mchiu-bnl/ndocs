# SimEvent data structure

The output of the nEXO DetSim simulation is a ROOT file containing a flat TTree with the following variables

Data members described below are defined in [SimEvent.h](../DataModel/SimEvent/Event/SimEvent.h).


Primary Event
------
* **fEventNumber**:  The ID used to identify different events.
* **fGenX**:  The X coordinate of event vertex (also the vertex of primary particles).
* **fGenY**:  The Y coordinate of event vertex (also the vertex of primary particles).
* **fGenZ**:  The Z coordinate of event vertex (also the vertex of primary particles).
* **fGenE**:  g4Track->GetKineticEnergy()/GeV;
* **fGenKineticE**: The kinetic energy of the primary particle as created by GPS (MeV)
* **fGenParticleID**: Returns a string of the particle name (proton, e+, e-, He3, neutron,...)  

Energy Deposits
------
* **fTotalEventEnergy**:  The total deposited energy in LXe for each event.
* **fNumDeposits**:  The total number of energy deposits in LXe for each event.
* **fEnergyDeposit**:  The array used to store deposited energy in each step in LXe, the length of array is NumDeposits.
* **fPreEnergyDeposit**:  The array used to store total energy before that step in LXe, the length of array is NumDeposits.
* **fPostEnergyDeposit**:  The array used to store total energy after that step in LXe, the length of array is NumDeposits.
* **fLengthDeposit**:  The array used to store length of the step in LXe, the length of array is NumDeposits.
* **fTrackNumber**:  The array used to store track IDs which contribute to energy deposits, the length of array is NumDeposits.
* **fParentTrackID**: The track ID number of the particle track that generated the track depositing energy, the length of array is NumDeposits.
* **fXpos**:  The array used to store X coordinate of each energy deposit, the length is NumDeposits.
* **fYpos**:  The array used to store Y coordinate of each energy deposit, the length is NumDeposits.
* **fZpos**:  The array used to store Z coordinate of each energy deposit, the length is NumDeposits.
* **fTglob**: The array used to store time of each energy deposit. This is the global time, ie the time between energy deposit and start of the event.
* **fTloc**: The array used to store time of each energy deposit. This is the local time, ie the time between energy deposit and start of the track.

Optical Photons
------
* **fInitNOP**:  The total number of optical photons generated in each event.
* **fInitCherenkovOP**: Total number of Cherenkov photons generated in the event.
* **fOPStopVolume**:  The array used to store volume ID that each optical photon stop on. The volume IDs are defined as:
   1→ ActiveRegion  
   2→ SiPMPads  
   3→ ActiveSiPMPads  
   4→ SiPM Module  
   5→ Field shaping rings  
   6→ Anode  
   7→ Cathode or Bulge  
   8→ TPC copper vessel  
   9→ Support Rods and Spacers
    -1→ Unknown (mostly SiPM sun=btrate pad)  
* **fNOP**:  The number of optical photons which are detected by SiPMs. 
* **fSiPMID**:  The array is used to store fired SiPM ID by each detected photon, the length is NumOP.
* **fOPEnergy**:  The array is used to store energy of each detected optical photon, the length is NumOP.
* **fOPTime**:  The array is used to store time that photons hit on SiPM, the length is NumOP.
* **fOPX**:  The array is used to store X coordinate on SiPM fired by different optical photons, the length is NumOP.
* **fOPY**:  The array is used to store Y coordinate on SiPM fired by different optical photons, the length is NumOP.
* **fOPZ**:  The array is used to store Z coordinate on SiPM fired by different optical photons, the length is NumOP.
* **fOPType**: The creater process for the optical photon. Defined as:
   1→ S1  
   2→ Cerenkov  
* **fAOI**:  angle of incident: to be updated to different branches for detection, absorption and reflection.
* **fNReflections**:  number of reflections of each optical photon of SiPM only. to be updated and renamed. 


Thermal Electrons
------
* **fNTE**: The number of thermal electrons created.

NEST
------
Each of the below variables stores a vector of values, with each entry in the vectors corresponding to a different NEST lineage. Each lineage represents a single decay or a single scatter by a gamma ray. Some decays or scatters have multiple child particles--for example, a double beta decay has two children, the betas; a photoelectric effect gamma sometimes produces multiple children, the main recoil electron + an x-ray + whatever that x-ray does; anything involving a fast electron may produce extra children due to a Bremsstrahlung gamma. Multiple children are (nearly always) bundled together into the same lineage.
* **fNESTLineageX**: X position of the lineage, averaged over the (unweighted) energy deposits in the lineage. [mm]
* **fNESTLineageY**: Y position of the lineage, averaged over the (unweighted) energy deposits in the lineage.
* **fNESTLineageZ**: Z position of the lineage, averaged over the (unweighted) energy deposits in the lineage. 
* **fNESTLineageXwidth**:  Furthest distance in X between any two energy deposits in this lineage. If this is large, there may be a flaw in the lineage-creation logic. [mm]
* **fNESTLineageYwidth**:  Furthest distance in Y between energy deposits.
* **fNESTLineageZwidth**:  Furthest distance in Y between energy deposits.
* **fNESTLineageNOP**:  Number of optical photons in the lineage.
* **fNESTLineageNTE**:  Number of thermal electrons in the lineage.
* **fNESTLineageE**:  Total energy deposit of the lineage [keV]
* **fNESTLineageT**:  Time of the lineage (unweighted mean of energy deposits) [ns]
* **fNESTLineageTwidth**:  Span between the first and last energy deposit in the lineage [ns.]
* **fNESTLineageType**: NEST type of the lineage.   xenon nucleus = 0,  ion/atom = 6,  photoelectric gammaRay = 7,   beta/compton = 8

* **fNESTHitX**: The array used to store X coordinate of each energy deposit or "Hit" by NEST, the length is fNESTHitType.
* **fNESTHitY**: The array used to store Y coordinate of each energy deposit or "Hit" by NEST, the length is fNESTHitType.
* **fNESTHitZ**: The array used to store Z coordinate of each energy deposit or "Hit" by NEST, the length is fNESTHitType. 
* **fNESTHitNOP**: The array used to store the optical photons produced by NEST for each "Hit", the length is fNESTHitType.
* **fNESTHitNTE**: The array used to store the thermal electrons produced by NEST for each "Hit", the length is fNESTHitType. 
* **fNESTHitE**: The array used to store the energy deposit of the "Hit" in NEST, the length is fNESTHitType. 
* **fNESTHitT**: The array used to store the time of the "Hit" in NEST, the length is fNESTHitType. 
* **fNESTHitType**: The array used to store the type of particle as defined by NEST in the "Hit". The partical Types are defined as:
   0→ Nuclear Recoil 
   1→ WIMP
   2→ B8
   3→ DD  
   4→ AmBe  
   5→ Cf 
   6→ ion (alphas, Pb206, etc.)
   7→ gamma ray
   8→ beta
   9→ CH3T  
   10→ C14 
   11→ Kr85m
   12→ None Type
   
BiPo decay variables
------
* **fTfirstDecay**: The time of the first Bi decay in global time.(Note: can be expanded to work for any decay)
* **fTreset**: The time of the energy deposit from the time of the first Bi decay (fTglob - fTfirstDecay), the length of array is NumDeposits. (Note: can be expanded to work for any decay)

Cosmogenics and Outer Detector variables
------
* **fGenTheta_p**: Generated angle from zenith of muons 
* **fGenPhi_p**: Generated phi angle of muons
* **fNeutronCount**:  Generated number of neutrons (for testing, soon to be removed)
* **fXe137Count**:  Generated nuber of Xe-137 atoms (Cuts down cosmogenics simulation file sizes)
* **fInitNeutronEnergy**:  Generated neutron energy
* **fPMT_Hits**:  Total number of PMT hits on photocathode (before QE)
* **fPMTID**:  ID number of PMT hits on photocathode (before QE)
* **fPMTs_fired**:  (deprecated, to be removed)
* **fmu_Impact_parameter**:  Distance of closest approach of muon to origin
* **fn_Impact_parameter**:  Distance of closest approach of neutron to origin
* **fTotalEventEnergy_WT**:  Total event energy deposited in the water
* **fEnergyDeposit_WT**:  Individual energy deposits in the water
* **fLengthDeposit_WT**:  length of energy deposits in the water
* **fNumDeposits_WT**:   Number of energy deposits in the water
* **fInitCherenkovOP**:  Number of Cherenkov photons produced

Clustering (DEPRECATED)
======
The output of the `Clustering.py` script is a ROOT file containing a flat TTree with the following variables. This tree is independent of that output by nEXO.

* **EventVertexX**: X position of the event vertex
* **EventVertexY**: Y position of the event vertex
* **EventVertexZ**: Z position of the event vertex
* **TotalEventEnergy**:  The total deposited energy inside the active LXe for each event.
* **NumClusters**: The number of clusters in event.
* **EnergyDeposit**: The energy of each cluster. This is the sum of the energy of its clustered G4 deposits.
* **Xpos**: The X position of each cluster. This is the average of the X position of its clustered G4 deposits weighted by their energies.
* **Ypos**: The Y position of each cluster. This is the average of the Y position of its clustered G4 deposits weighted by their energies.
* **Zpos**: The Z position of each cluster. This is the average of the Z position of its clustered G4 deposits weighted by their energies.

Reconstruction (DEPRECATED)
======
The output of the Reconstruction.py script is a ROOT file containing a flat TTree with the same variables from the `Clustering.py` (above), in addition to the following below. 

* **ReconFlag**: Integer flag that identifies in which of the below categories the event fall:  
   2 = successfully reconstructed  
   1 = below V-threshold  
   0 = below U-threshold  
   -1 = at least one cluster above U-threshold found outside of the FV
* **TotalReconEnergy**:  The total energy reconstructed for events satisfying the FV and the energy thresholds cuts.
* **NumReconClusters**: The number of reconstructed clusters after FV and energy cuts.
* **SmearedEnergy**: The reconstructed energy after detector effects of smearing.
* **StandoffDistance**: Evaluated standoff distance for clusters above U-threshold and inside of the FV.

This tree is still connected to the output by `Clustering.py` and both can be found in the output file of the reconstruction. However, because they are connected, the intersection branches do not take extra disk space.

AddDecayChainInfo (DEPRECATED) 
======
The following variables are added to the output trees of the above processes once the `AddDecayChainInfo.py` is run for them. The new tree is connected to the previous tree(s) and the its name is the same with the suffix "Decay".

* **ParentZ**: The atomic number of the parent of the decay chain.
* **ParentA**: The atomic mass of the parent of the decay chain.
* **NuclideZ**: The atomic number of the isotope listed in the decay chain that generated the event.
* **NuclideA**: The atomic mass of the isotope listed in the decay chain that generated the event.
* **BranchFraction**: The branch fraction (given in card) of the isotope that generated the event.
* **Weight**: Event weight considering the branch fraction and the success rate of the simulations (if more than one job is requested).
