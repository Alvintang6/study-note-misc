

### 1.Istallation of GPU driver 

Most of the time, the GPU driver is installed by default. In case the changing of GPU. People can install the GPU driver.
The GPU driver is installed under the system of win10/win11，choosing you r driver from the list https://developer.nvidia.com/cuda/wsl.

### 2.在WSL2中安装CUDA
Before installing the CUDA make sure you have the GCC installed.

```shell 
sudo apt-get install gcc
```

Following the steps to install CUDA. Since we already have nvidia gpu driver，we only need to install cuda-toolkit.

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda-repo-wsl-ubuntu-11-1-local_11.1.1-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local_11.1.1-1_amd64.deb
sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install -y cuda-toolkit-11-1
```


After installation，the default CUDA installation path is under /usr/local，changing your own path by needs.

adding following command into the ```vim ~/.bahsrc``

```shell
export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
CUDA_HOME=/usr/local/cuda-11.1; export CUDA_HOME
export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

### 3.安装 NVIDIA HPC SDK

Before installing the HPC SDK checking your g++ installation.

```shell
sudo apt-get install g++
```

Download the NVIDIA HPC from NVIDIA offical website https://developer.nvidia.com/hpc-sdk (Here I choosing 20.11 which released on 2020.Dec)：

```shell
$ wget https://developer.download.nvidia.com/hpc-sdk/20.11/nvhpc_2020_2011_Linux_x86_64_cuda_11.1.tar.gz
$ tar xpzf nvhpc_2020_2011_Linux_x86_64_cuda_11.1.tar.gz
$ nvhpc_2020_2011_Linux_x86_64_cuda_11.1/install
```

After installing HPC SDK，the default installation path of HPC SDK is /opt/nvidia，changing the path with your own software SDK version(here is 20.11 for example)。

```shell
NVARCH=`uname -s`_`uname -m`; export NVARCH
NVCOMPILERS=/opt/nvidia/hpc_sdk; export NVCOMPILERS
MANPATH=$MANPATH:$NVCOMPILERS/$NVARCH/20.11/compilers/man; export MANPATH
PATH=$NVCOMPILERS/$NVARCH/20.11/compilers/bin:$PATH; export PATH
```

end of the installation.
