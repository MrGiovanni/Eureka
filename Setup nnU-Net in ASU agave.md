# nnU-Net in ASU agave

Acknowlegement: We thank Shivam Bajpai for reproducing nnU-Net. Thank Fabian Isensee for building this fantastic nnU-Net project.

```bash
module load anaconda/py3
conda create -n env_name python=3.7
source activate env_name
conda install pytorch torchvision cudatoolkit=10.2 -c pytorch
pip install --upgrade batchgenerators hiddenlayer Ipython graphviz

git clone https://github.com/NVIDIA/apex
cd apex
sbatch --error=apex_install.out --output=apex_install.out apex_model.sh

cd nnUNet/nnunet
python paths.py
cd nnUnet/
pip install -e .

sbatch --error=logs/plan.out --output=logs/plan.out plan.sh

sbatch --error=logs/lung0.out --output=logs/lung0.out lung_mode0.sh

tail -F logs/
```
