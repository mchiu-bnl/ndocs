1. First Select only events with total event energy > 1000 keV (?) and at least one deposit in the FV (will need to define FV)

   a. Charge reconstruction:

      * Start from MC number of TE. These are stored in ElecEvent and ElecChannels
      * Sum TE for all channels
      * Apply electronic noise. Use value arising from Optimal Filter, 100e- noise assumption, and Aldo's power spectrum. We will discuss where these values come from and comment on the fact that the optimal filter is the ideal best limit. We will also determine what integrated noise level is compatible with 1%. This can come from realistic filter and/or worse-than-predicted electronic noise.
      * This is most certainly resulting in < 1 % energy resolution.  This is acceptable IMO. We will refer or regenerate a plot of sensitivity vs energy resolution to show how little it would change if we were to have 1% energy resolution instead

   b. Scintillation reconstruction:

      * Start from  MC number of OP for each hit. These are in SimEvent as NESTHitNOP vector.
      * Compute the PTE for each event as the weighted sum of the OP in each hit. Efficiencies are loaded from a lightmap for the Skin volume. We can assume the lightmap has been calibrated with certain level of accuracy within the FV.
        * What lightmap should we use? One with average FV efficiency of 3% or 7%. I prefer the former.
        * What level of accuracy should we assume for the light map calibration?
      * Compute the collected light and apply binomial fluctuation.
      * Compute the number of PE from by applying the PDE fraction and applying binomial fluctuation.
        * What value of PDE should we assume? 15% seems defensible
      * NB: there is no threshold! All photons are included.
      * Another assumption here is that there is no saturation (this is likely true for gamma interactions)

   c. Energy: From charge/light rotation

   d. StandOff calculation:

      * Start from MC data stored in the ElecEvent (fmc_tepos) or from the tiles X,Y coordinates
      * Compute standoff as in the past.  Need to define the boundary surface. Alternatively, we could compute the distance from the center of the TPC.  If we want to be precise, we could account for the fact that the field cage diameter is not exactly equal to the height.
        * What threshold?

   e. DNN parameter:

      * This is computed with a separate python script until we can get pytorch in Sniper --> perhaps Heather can help?


2. What event selection is used to fill the PDFs that are sent to the sensitivity fitting code?
   
   a. Should we apply C/L cut?
