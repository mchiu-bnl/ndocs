# Submitting Slurm jobs in SDF (SLAC)

To submit a Slurm job, you first need to make sure you have an active SLAC Windows account. 
You can check here: https://oraweb.slac.stanford.edu/apex/slac/f?p=136

Once you have verified that you have an active Windows account, you can ssh into the "Shared Data Facility" (SDF). For more info on the new SDF, see: https://ondemand-dev.slac.stanford.edu. 
You can ssh into SDF with:
* `ssh <username>@sdf-login.slac.stanford.edu`

Note that the SDF space is separate from the NFS space, so you will not be able to access stuff in your NFS directory. However, a nEXO directory has already been set up in SDF, here: `/sdf/group/nexo`

So, to submit a Slurm job, you first need to build nexo-offline in your SDF space, as shown here: https://github.com/nEXO-collaboration/nexo-offline/blob/documentation_update/Doc/singularity.md

Notice that you will need to replace the line `singularity shell --home $PWD /nfs/slac/g/nexo/software/prod/singularity/nexo-base_v4r1p0.sif` with `singularity shell --home $PWD /sdf/group/nexo/software/prod/singularity/nexo-base_v4r2p0.sif`. Else the code is identical. 

Now we are ready to write the scripts for submitting Slurm jobs. 

## Scripts

From your home directory in SDF, make the following files: 

* runnexo.sh

```
#!/bin/bash
echo $PWD
cd /opt/nexo/software
source bashrc.sh
source sniper-install/setup.sh
cd ~/nexo-offline-build
source setup.sh
cd Cards
python ./RunDetSim.py --evtmax 100 --seed 1 --run ./examples/TPCVesselU.mac --output TPCVesselU-slurmTest.root > TPCVesselU-slurmTest.out 2>TPCVesselU-slurmTest.err
```

* slurm_test.sh

```
#!/bin/bash
#SBATCH --account=shared --partition=shared
#
#SBATCH --job-name=slurmJobTest
#SBATCH --output=output-%j.txt --error=output-%j.txt
#
#SBATCH --nodes=1 --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1g
#SBATCH --time=0:30:00
#
# cd /sdf/home/<yourSDFdirectory>
singularity exec --home $PWD /sdf/group/nexo/software/prod/singularity/nexo-base_v4r2p0.sif ./runnexo.sh
```

Notice that '#' does not mean that these lines are commented out, it is just Slurm format. 

Lastly, before we submit the job, we need to set the permissions appropriately with `chmod u+x runnexo.sh`.

After these two scripts have been made and the permissions have been set, you may submit your job by doing: 
```
sbatch slurm-test.sh
```

Other useful Slurm commands are: 

* `scontrol show job <jobnumber>` to check the status of your job

* `squeue -u <username>` to see all the active jobs you have

* `scancel <jobid>` to cancel the job

* `scancel -u <username>` to cancel all the jobs for a user

* `scancel -t PENDING -u <username>` to cancel all the pending jobs for a user

More info on Slurm jobs can be found here (from slide 28):  https://confluence.slac.stanford.edu/download/attachments/213897042/Containers%20and%20GPUs.pdf?version=1&modificationDate=1588277839000&api=v2
