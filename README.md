# CIS6930-Final-Project


=========================================================
         ENVIRONMENT AND KERNEL SETUP GUIDE
=========================================================

This project uses a custom Conda environment and Jupyter
kernel configured for PyTorch with CUDA (GPU) support.
Follow these steps to recreate the same setup used for
development on HiPerGator.

---------------------------------------------------------
STEP 1: Load Conda on HiPerGator
---------------------------------------------------------
module load conda

---------------------------------------------------------
STEP 2: Create the Conda Environment
---------------------------------------------------------
conda create -n llama3 python=3.10 -y
conda activate llama3

---------------------------------------------------------
STEP 3: Install Required Packages
---------------------------------------------------------
# Install PyTorch with CUDA support
conda install -c pytorch -c nvidia pytorch torchvision torchaudio pytorch-cuda=12.1 -y

# Install Hugging Face and related libraries
conda install -c conda-forge transformers datasets accelerate evaluate tokenizers safetensors -y

# Install optimization and fine-tuning tools
conda install -c conda-forge bitsandbytes peft trl -y

# Install Jupyter and kernel tools
conda install jupyter ipykernel -y

NOTE:
If bitsandbytes gives a CUDA error later, make sure
you are running your notebook on a GPU node.

---------------------------------------------------------
STEP 4: Register the Jupyter Kernel
---------------------------------------------------------
python -m ipykernel install --user --name llama3-cuda --display-name "llama3 (PyTorch CUDA)"

After this, the kernel should appear in JupyterLab as:
llama3 (PyTorch CUDA)

---------------------------------------------------------
STEP 5: Verify Installation (Optional)
---------------------------------------------------------
Run the following test cell inside Jupyter:

import torch, transformers, bitsandbytes
print("Torch:", torch.__version__)
print("Transformers:", transformers.__version__)
print("BitsAndBytes:", bitsandbytes.__version__)
print("CUDA available:", torch.cuda.is_available())

Expected output:
Torch: 2.x.x
Transformers: 4.x.x
BitsAndBytes: 0.x.x
CUDA available: True

If CUDA available shows False, ensure the notebook
is running on a GPU node.

---------------------------------------------------------
STEP 6: Recreate Environment from environment.yml (Optional)
---------------------------------------------------------
If this repository includes an environment.yml file,
you can rebuild everything automatically instead:

conda env create -f environment.yml
conda activate llama3
python -m ipykernel install --user --name llama3-cuda --display-name "llama3 (PyTorch CUDA)"

---------------------------------------------------------
DONE!
---------------------------------------------------------
You can now open the CIS6930_Final_Project.ipynb notebook
and select the kernel:
llama3 (PyTorch CUDA)

=========================================================
