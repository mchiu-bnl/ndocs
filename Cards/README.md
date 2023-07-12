Usage
-----
Create bash scripts
```bash
python SubmitSimulation.py -c yamls/jobs.yaml
```

Submit to queue
```bash
python SubmitSimulation.py -c yamls/jobs.yaml -s
```

Format
-----
Cards are in YAML format. Settings are divided into sections. 
- System: Settings for the platform where the jobs are run.
- DetectorSimulation: Settings for GEANT4. Mostly for geometry definition and event generation.
- ChargeSimulation: Settings for charge simulation
- FastLightSimulation: *work in progress*
- Reconstruction: Setting for reconstruction
- Merge: Settings for merging recon sniper-trees into plain ROOT trees suitable for sensitivity calculation and materials database.


References for YAML
-----
- Primer: https://github.com/darvid/trine/wiki/YAML-Primer
- Official site: https://yaml.org/
- Cheatsheet: https://yaml.org/refcard.html
