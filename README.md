# nvidia-ubuntu-24-04
Guide Nvidia 555 driver, CUDA toolkit 12.5, and cuDNN 9.2.1 on Ubuntu 24.04.
# NVIDIA Setup Guide for Ubuntu 24.04

This guide provides step-by-step instructions to install NVIDIA drivers, CUDA toolkit, and cuDNN on Ubuntu 24.04. It also includes tips for verifying installations and troubleshooting.

## Steps to Install

### 1. Disable Wayland and Nouveau

#### Disable Wayland
Edit `/etc/gdm3/custom.conf` and set `WaylandEnable=false`.

#### Blacklist Nouveau
```bash
sudo bash -c 'echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf'
sudo bash -c 'echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf'
sudo bash -c 'echo "blacklist nvidia-drm" >> /etc/modprobe.d/blacklist.conf'
sudo bash -c 'echo "blacklist nvidia-modeset" >> /etc/modprobe.d/blacklist.conf'
sudo update-initramfs -u
sudo reboot
```

### 2. Install NVIDIA Driver

#### Add the NVIDIA PPA and Update
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```

#### Install the NVIDIA Driver
```bash
sudo apt install nvidia-driver-555 -y
sudo reboot
```

### 3. Install CUDA Toolkit

#### Download the CUDA Repository Package and Install
Refer to the official [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive) to find the latest version for Ubuntu 24.04.

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-repo-ubuntu2404_12.5.1-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2404_12.5.1-1_amd64.deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/7fa2af80.pub
sudo apt update
sudo apt install cuda -y
```

### 4. Install cuDNN

Refer to the official [cuDNN Archive](https://developer.nvidia.com/cudnn-archive) to find the version you need.

#### Extract and Copy Files
```bash
tar -xzvf cudnn-9.2.1-linux-x64-v9.2.1.18.tgz
sudo cp cuda/include/cudnn*.h /usr/include
sudo cp cuda/lib64/libcudnn* /usr/lib/x86_64-linux-gnu
sudo ldconfig
```

### 5. Setup Environment Variables

#### Edit `~/.bashrc`
```bash
nano ~/.bashrc
```

#### Add the Following Lines
```bash
export PATH=/usr/local/cuda-12.5/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.5/lib64:$LD_LIBRARY_PATH
export CUDNN_INCLUDE_DIR=/usr/include
export CUDNN_LIB_DIR=/usr/lib/x86_64-linux-gnu
```

#### Source the Updated `~/.bashrc`
```bash
source ~/.bashrc
```

### 6. Verify Installations

#### Verify NVIDIA Driver
```bash
nvidia-smi
```

#### Verify CUDA Toolkit
```bash
nvcc --version
```

#### Verify cuDNN
```bash
ls /usr/include/cudnn*.h
ls /usr/lib/x86_64-linux-gnu/libcudnn*
```

### 7. Run Tests

#### Install FreeImage Library
```bash
sudo apt-get install libfreeimage-dev
```

#### Compile and Run a cuDNN Sample
```bash
cp -r /usr/src/cudnn_samples_v9 $HOME/
cd $HOME/cudnn_samples_v9/mnistCUDNN
make clean && make
./mnistCUDNN
```

## Troubleshooting Tips

### Finding Libraries
If the directories `/usr/include` or `/usr/lib/x86_64-linux-gnu` do not contain the expected files, use the `find` command to locate them:
```bash
sudo find / -name "cudnn*.h"
sudo find / -name "libcudnn*"
```

### Adjusting Paths
If libraries are found in different directories, adjust the paths in the environment variable setup accordingly.

## Latest Versions

- **NVIDIA Driver**: [NVIDIA Drivers](https://www.nvidia.com/Download/index.aspx)
- **CUDA Toolkit**: [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)
- **cuDNN**: [cuDNN Downloads](https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu)

By following these steps, you can ensure a successful installation of NVIDIA drivers, CUDA toolkit, and cuDNN on Ubuntu 24.04.
