# ElecEvent data structure

The output of the Digitization charge simulation is a ROOT file containing a TTree called `ElecEvent` 
(located in the TDirectory path `Event/Elec/ElecEvent`)

Data members described below are defined in [ElecEvent.h](../DataModel/ElecEvent/Event/ElecEvent.h) and 
[ElecChannel.h](../DataModel/ElecEvent/Event/ElecChannel.h).

Primary Event
------
* **fNTE**:  Total number of thermal electrons generated in the event (should mirror **fNTE** in the `SimEvent` tree)
* **fEnergy**: Total energy deposited in LXe in the event (should mirror **fTotalEventEnergy** in the `SimEvent` tree)

Channel information
------
The data for each channel (i.e. each strip) is contained in the `ElecChannel` data structure,
which is defined in [ElecChannel.h](../DataModel/ElecEvent/Event/ElecChannel.h). The simulation
records three types of channels: those which collect charge, those which do not collect charge
but see a significant induction signal, and a set of randomly-generated noise-only channels.

* **fTileId**: the tile containing this channel. ID numbers are defined by the tilesMap (e.g. [tilesMap_6mm.txt](../data/tilesMap_6mm.txt))
* **fxTile**: X-position of the given tile. Defined by the tilesMap, which lists the center positions of each tile
* **fyTile**: Y-position of the given tile. Defined by the tilesMap, which lists the center positions of each tile
* **fXPosition**: X-position of the specific strip. Defined by the tilesMap and the localChannelsMap (e.g. [tilesMap_6mm.txt](../data/tilesMap_6mm.txt) and [localChannelsMap_6mm.txt](../data/localChannelsMap_6mm.txt))
* **fYPosition**: Y-position of the specific strip. Defined by the tilesMap and the localChannelsMap
* **fChannelLocalId**: the ID number of the specific strip within a tile. ID numbers are defined by the localChannelsMap (e.g. [localChannelsMap_6mm.txt](../data/localChannelsMap_6mm.txt))
* **fChannelCharge**: the charge detected by the channel.
* **fChannelInductionAmplitude**: *DEPRECATED*
* **fChannelFirstTime**: *DEPRECATED*
* **fChannelLatestTime**: *DEPRECATED*
* **fChannelTime**: *DEPRECATED*
* **fChannelNTE**: Is redundant with fChannelCharge
* **fChannelNoiseTag**: boolean, true if this is one of the randomly-generated noise channels. False if there is actually a collected or induced signal
* **fInductionAmplitude**: *DEPRECATED*
* **fNoiseOn**: *DEPRECATED*
* **fWFLen**: length of the waveform in samples
* **fWFChannelCharge**: Is redundant with fChannelCharge and fChannelNTE.
* **fTrueWF**: contains the actual simulated noiseless-waveform for this channel. Note that the waveform amplitude is divided a factor of 9 from the true current* **
* **fWFAndNoise**: contains the noisy waveform for this channel. Note that the waveform amplitude is divided a factor of 9 from the true current* **


\* The factor of 9 is to increase the dynamic range of the `int` data type

\** This data includes the predicted ADC gain of nEXO (3.03) which must be divided out to get units of electrons




