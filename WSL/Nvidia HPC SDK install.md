

### 1.Istallation of GPU driver 

Most of the time, the GPU driver is installed by default. In case the changing of GPU. People can install the GPU driver.
The GPU driver is installed under the system of win10/win11，choosing you r driver from the list https://developer.nvidia.com/cuda/wsl.

### 2.Install CUDA inside WSL
Before installing the CUDA make sure you have the GCC installed.

```shell 
sudo apt-get install gcc
```

Following the steps to install CUDA. Since we already have nvidia gpu driver，we only need to install cuda-toolkit.

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
```


After installation，the default CUDA installation path is under /usr/local，changing your own path by needs.

adding following command into the ```vim ~/.bahsrc``

```shell
export PATH=/usr/local/cuda-12.0/bin${PATH:+:${PATH}}
CUDA_HOME=/usr/local/cuda-12.0; export CUDA_HOME
export LD_LIBRARY_PATH=/usr/local/cuda-12.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

### 3. Install NVIDIA HPC SDK

Before installing the HPC SDK checking your g++ installation.

```shell
sudo apt-get install g++
```

Download the NVIDIA HPC from NVIDIA offical website https://developer.nvidia.com/hpc-sdk (Here I choosing 20.11 which released on 2020.Dec)：

```shell
sudo apt-get install gfortran
$ wget https://developer.download.nvidia.com/hpc-sdk/23.1/nvhpc_2023_231_Linux_x86_64_cuda_12.0.tar.gz
$ tar xpzf nvhpc_2023_231_Linux_x86_64_cuda_12.0.tar.gz
$ nvhpc_2023_231_Linux_x86_64_cuda_12.0/install
```

After installing HPC SDK，the default installation path of HPC SDK is /opt/nvidia，changing the path with your own software SDK version(here is 20.11 for example)。

```shell
NVARCH=`uname -s`_`uname -m`; export NVARCH
NVCOMPILERS=/opt/nvidia/hpc_sdk; export NVCOMPILERS
MANPATH=$MANPATH:$NVCOMPILERS/$NVARCH/23.1/compilers/man; export MANPATH
PATH=$NVCOMPILERS/$NVARCH/23.1/compilers/bin:$PATH; export PATH
```

```shell
In bash, sh, or ksh, use these commands:

MANPATH=$MANPATH:/opt/nvidia/hpc_sdk/Linux_x86_64/23.1/compilers/man; export MANPATH PATH=/opt/nvidia/hpc_sdk/Linux_x86_64/23.1/compilers/bin:$PATH; export PATH

export PATH=/opt/nvidia/hpc_sdk/Linux_x86_64/23.1/comm_libs/mpi/bin:$PATH
export MANPATH=$MANPATH:/opt/nvidia/hpc_sdk/Linux_x86_64/23.1/comm_libs/mpi/man

```

end of the installation.
