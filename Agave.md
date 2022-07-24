# Connect to ASU VPN

##### 0. Download Cisco AnyConnect Secure Mobility Client from [myapps.asu.edu](https://myapps.asu.edu/app/cisco-ssl-vpn-0) and install it on your computer

##### 1. Open Cisco AnyConnect Secure Mobility Client
Type in: `sslvpn.asu.edu` and click `Connect`

##### 2. Type in user name (`zzhou82`), password, and authentication (`phone')

# Access ASU GPU Cluster
- Credit to [ASU Research Computing](https://rcstatus.asu.edu/agave/)
- For any assistance please contact: [support@hpchelp.asu.edu](support@hpchelp.asu.edu)
##### Create an account at https://cores.research.asu.edu/research-computing/about-rc then you are good to go

```bash
ssh -Y zzhou82@agave.asu.edu
scp /Users/zongwei.zhou/debug.py zzhou82@agave.asu.edu:/home/zzhou82/zongwei.zhou/ # Transferring files
pip install simpleitk photutils tifffile libtiff pydot --user
sbatch --error=logs/run_resnet50.out --output=logs/run_resnet50.out run_script.sh resnet50 # Submit a job
squeue -u zzhou82 # Print the current job list
myjobs # Print the current job list in detail
myjobs | wc -l # Count the total number of current jobs (maximum 1000 jobs allowed per users)
scancel *** (JOBID)
```

##### An example of shell scripts (in run_script.sh)
```
#!/bin/bash
#SBATCH -N 1
#SBATCH -n 8
##SBATCH --mem-per-cpu 50000
##SBATCH -p physicsgpu1
##SBATCH -p sulcgpu2
##SBATCH -p mrlinegpu1
#SBATCH -p asinghargpu1
##SBATCH -p sulcgpu1
##SBATCH -p cidsegpu1
#SBATCH -q wildfire
#SBATCH --gres=gpu:1
#SBATCH -t 9-23:59:59
##SBATCH -o slurm.%j.${1}.out
##SBATCH -e slurm.%j.${1}.err
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=zzhou82@asu.edu

module load tensorflow/1.8-agave-gpu
module unload python/.2.7.14-tf18-gpu

# autoencoder
python3.6 -W ignore Genesis_ImageNet_2D.py --note autoencoder --rescale_rate 0.1 --flip_rate 0.01 --rotation_rate 0.01 --transpose_rate 0.01 --gamma_rate 0.0 --paint_rate 0.0 --outpaint_rate 0.8 --local_rate 0.0 --nonlinear_rate 0.0 --twist_rate 0.0 --blur_rate 0.0 --arch Unet --decoder upsampling --backbone $1 --batch_size 24 --input_rows 224 --input_cols 224 --input_deps 3 --crop_rows 512 --crop_cols 512 --crop_deps 3 --steps_per_epoch 5000 --worker 4 --init finetune --nb_class 3 --verbose 1 --optimizer sgd --save_samples png --data /home/zzhou82/zongwei.zhou/Models-Genesis/ImageNet/ILSVRC2012
```
More examples and explanations can be found at https://rcstatus.asu.edu/agave/howto/gpu.php
```
#!/bin/bash

#SBATCH -N 1                        # number of compute nodes
#SBATCH -n 1                        # number of CPU cores to reserve on this compute node

#SBATCH -p gpu                      # This is the recommended default partition
###SBATCH -p cidsegpu1              # Uncomment this line to specifically callout the cidsegpu1 partition instead

#SBATCH -q wildfire                 # Run job under wildfire QOS queue

#SBATCH --gres=gpu:2                # Request two GPUs

#SBATCH -t 0-12:00                  # wall time (D-HH:MM)
#SBATCH -o slurm.%j.out             # STDOUT (%j = JobId)
#SBATCH -e slurm.%j.err             # STDERR (%j = JobId)
#SBATCH --mail-type=ALL             # Send a notification when the job starts, stops, or fails
#SBATCH --mail-user=myemail@asu.edu # send-to address
```

Check the graphical view of Agave here: https://rcstatus.asu.edu/agave/smallstatus.php

##### Shared folder with unlimited space in ASU GPU:
zzhou82@agave.asu.edu:/scratch/zzhou82/

##### Transfer files to Google Drive
- Documentation: https://asurc.atlassian.net/wiki/spaces/RC/pages/349175850/Configuring+Globus+Collections
