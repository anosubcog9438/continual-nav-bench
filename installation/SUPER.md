# **Installation** ( Linux SuperComputer - e.g. [Archer 2](https://www.archer2.ac.uk/) , [Jean Zay](http://www.idris.fr/ia/) , [Stampede 3](https://tacc.utexas.edu/systems/stampede3/) , [Magic Cube III](https://www.ssc.net.cn/en/about-history.html) )

___
___

[ The commands are to be executed in a bash session. ]

It is advised to create a workspace in your work directory that will contain all needed repository and data :

( `git clone` may not work depending on the os configuration, you would have to directly copy the folder from the main branch )

```bash
cd $WORK
mkdir -p workspace_continual
```

From now on, it is supposed we have such a workspace ( `$WORK/workspace_continual` )

As we use [Hydra](https://hydra.cc/docs/intro/) it is advised to verify (and to modify) the paths considered in the `.yaml` configurations files.

## **1. Python**

### **1.A. Python Installation**

Follow the instructions in the provided links to install the latest version of either [Anaconda](https://docs.anaconda.com/free/anaconda/install/windows/), [Miniconda](https://docs.anaconda.com/free/miniconda/index.html), or [Miniforge](https://github.com/conda-forge/miniforge).

Here is how to simply do it with **Miniforge** :

```bash
cd $WORK
mkdir -p downloads
cd downloads/
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
chmod +x *
bash Miniforge3-Linux-x86_64.sh
```

The installation should be done after following the prompts and refreshing the terminal.

### **1.B. Conda Channels Setup**

Create a `.condarc` file in your home directory ( `/home/<YOU>/` ) and edit it with thie following content :

```bash
channels:
    - conda-forge
```

Add additional channels for package installation :

```bash
conda config --env --add channels nvidia
conda config --env --add channels pytorch
```

And update conda :

```bash
conda update -n base -c conda-forge conda
```

Here is a command to show the channels :

```bash
conda config --show channels
```

The output should be :

```bash
channels:
  - conda-forge
  - pytorch
  - nvidia
```

### **1.C. Conda Environment Setup**

It is advised to create a workspace containing all needed repository and data :

And to create a new environment :

```bash
conda create -n offbench python=3.10.14 -y
conda activate offbench
```

And **only once** in the environment :

```bash
conda install -c conda-forge pip -y
conda install -c nvidia cuda-toolkit=12.1 -y
conda install -c conda-forge --override-channels mesalib=24.3.1 glew=2.1.0 glfw=3.4 -y
conda install -c conda-forge --override-channels libgcc=14.2.0 -y
conda install -c conda-forge --override-channels patchelf=0.17.2 -y
conda install -c conda-forge --override-channels libglvnd-glx-cos7-x86_64=1.0.1 -y --force-reinstall
conda install -c conda-forge --override-channels xvfbwrapper=0.2.9 -y 
conda install -c conda-forge --override-channels xorg-libx11==1.8.10 -y
conda install -c conda-forge --override-channels xorg-xorgproto==2024.1 -y
```

From now on, it is supposed we are working within such an environment (`offbench`).

To avoid using all the memory available in the `$HOME` directory, do as it follows :

```bash
cd $WORK
mkdir -p pip-cache
```

Then add this line to the `.bashrc` file and reload the terminal :

```bash
export PIP_CACHE_DIR="$WORK/pip-cache/"
```

And run these commands :

```bash
conda install -c conda-forge pip
conda install -c nvidia cuda-toolkit=12.1
python -m pip install --upgrade pip
```

## **2. Install Current Library**

Follow these commands :

```bash
cd $WORK
cd ./workspace_continual/
git clone https://github.com/anosubcog9438/continual-nav-bench.git
```

## **3. Required Packages & Libraries**

### **3.A. Requirements**

Follow these commands :

```bash
cd $WORK
cd ./workspace_continual/continual-nav-bench/python/
pip instal -r requirements.txt
pip instal -e .
```

### **3.B. Pytorch**

Run this command (to install the CUDA 12.1 version) :

```bash
python -m pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
```

## **4. Datasets**

Follow the [README](../README.md) instructions.

## **5. Godot**

To allow evaluation on the Godot based environments (it may be possible that you have to do it every time you pull from the main branch..) :

```bash
cd $WORK/workspace_continual/continual-nav-bench/python/envs/godot_goal/executables/
chmod +x *
```

___
___
