# Important bash command lines you should know

# Connect to ASU VPN
### 1. Open Cisco AnyConnect Secure Mobility Client
Type in: `sslvpn.asu.edu` and click `Connect`

### 2. Type in user name (asu email) and password

### 3. Remotely access the lab machines
```bash
ssh zongwei@t4-host.dhcp.asu.edu -X
exit
scp -r zongwei@t4-host.dhcp.asu.edu:/mnt/dfs/zongwei /Users/zongwei.zhou/
scp zongwei@t4-host.dhcp.asu.edu:/mnt/dfs/zongwei/debug.py /Users/zongwei.zhou/
```

# Access ASU GPU cluster
```bash
ssh -Y zzhou82@agave.asu.edu
scp /Users/zongwei.zhou/debug.py zzhou82@agave.asu.edu:/home/zzhou82/zongwei.zhou/
pip install simpleitk photutils tifffile libtiff pydot --user
sbatch --error=logs/run_resnet50.out --output=logs/run_resnet50.out run_script.sh resnet50
squeue -u zzhou82
scancel *** (JOBID)
vi run_script.sh
```

### Shared folder with unlimited space in ASU GPU:
zzhou82@agave.asu.edu:/scratch/zzhou82/

