# nnU-Net in ASU agave

Acknowlegement: We thank Shivam Bajpai for reproducing nnU-Net. Thank Fabian Isensee for building this fantastic nnU-Net project.


### New version:
```bash
module load anaconda/py3
mkdir environments
cd environments
python3 -m venv nnunet
source nnunet/bin/activate
pip3 install torch torchvision torchaudio

git clone https://github.com/MrGiovanni/UNetPlusPlus.git
cd UNetPlusPlus/pytorch
pip install git+https://github.com/MIC-DKFZ/batchgenerators.git

cd nnunet
vi paths.py
#base = os.environ['nnUNet_raw_data_base'] if "nnUNet_raw_data_base" in os.environ.keys() else None
#preprocessing_output_dir = os.environ['nnUNet_preprocessed'] if "nnUNet_preprocessed" in os.environ.keys() else None
#network_training_output_dir_base = os.path.join(os.environ['RESULTS_FOLDER']) if "RESULTS_FOLDER" in os.environ.keys() else None

base = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch"
preprocessing_output_dir = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch/nnUNet_preprocessed"
network_training_output_dir_base = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch/RESULTS_FOLDER"

python paths.py

pip install --upgrade git+https://github.com/FabianIsensee/hiddenlayer.git@more_plotted_details#egg=hiddenlayer

cd pytorch
pip install --upgrade setuptools torchsummary ipython graphviz
pip install -e .

nnUNet_plan_and_preprocess -t XXX --verify_dataset_integrity
```

### Adjust the depth of nnU-Net
```bash
cp -r nnUNet nnUNetDep1
cd nnUNetDep1/nnunet
vi paths.py

base = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch"
preprocessing_output_dir = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch/nnUNet_preprocessed"
network_training_output_dir_base = "/home/zzhou82/zongwei.zhou/UNetPlusPlus/pytorch/RESULTS_FOLDER"

python paths.py

cd nnUNetDep1
pip install -e .

vi change_depth.py

plan['plans_per_stage'][1]['conv_kernel_sizes'] = [[3,3,3],[3,3,3],[3,3,3]]
plan['plans_per_stage'][1]['pool_op_kernel_sizes'] = [[2,2,2],[1,2,2]]

python change_depth.py --file nnUNet_preprocessed/Task555_FLARE/nnUNetPlansv2.1_plans_3D.pkl

CUDA_VISIBLE_DEVICES=1 nnUNet_train 3d_fullres nnUNetTrainerV2 Task555_FLARE 0
```

### Old version:
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
