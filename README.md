# nvidia-deepstream-quickstart

Requirements

* All done in a ubuntu (native - not a Virtual Machine). Version 20.04, other versions may work.
* NVIDIA GPU

Deepstream example list:

* https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_C_Sample_Apps.html


# Confirm setup

## Confirm setup - NVIDIA Driver

Check your driver is ok by running the `nvidia-smi` command. If this doesn't work, fix it first as subsequent stages won't work.

```bash
# Command:
nvidia-smi

# Response:
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 510.73.05    Driver Version: 510.73.05    CUDA Version: 11.6     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA RTX A5000    Off  | 00000000:0A:00.0  On |                  Off |
| 30%   44C    P5    30W / 230W |   1098MiB / 24564MiB |     36%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1329      G   /usr/lib/xorg/Xorg                485MiB |
|    0   N/A  N/A      3418      G   xfwm4                               4MiB |
|    0   N/A  N/A      3561      G   ...mviewer/tv_bin/TeamViewer       33MiB |
|    0   N/A  N/A     92497      G   /opt/zoom/zoom                     29MiB |
|    0   N/A  N/A     92538      G   .../debug.log --shared-files        3MiB |
|    0   N/A  N/A    107961      G   ...Debug/ground-truth-mapper      261MiB |
|    0   N/A  N/A    140112      G   ...AAAAAAAAA= --shared-files       58MiB |
|    0   N/A  N/A    461588      G   ...RendererForSitePerProcess       12MiB |
|    0   N/A  N/A   1823286      G   /usr/lib/firefox/firefox          143MiB |
|    0   N/A  N/A   1850911      G   ...AAAAAAAAA= --shared-files       16MiB |
|    0   N/A  N/A   3529797      G   qtcreator                           4MiB |
|    0   N/A  N/A   4155163      G   /opt/yEd/jre/bin/java              34MiB |
+-----------------------------------------------------------------------------+

```

If the above doesn't work, some of the following might be helpful:

```bash

# Check which version of the driver you have installed:

# Command:
dpkg -l | grep nvidia-driver

# Response:
ii  nvidia-driver-510                                           510.73.05-0ubuntu0.20.04.1           amd64        NVIDIA driver metapackage


# Check which versions are available:

# Command:
apt list "nvidia-driver*"

# Response:
Listing... Done
nvidia-driver-390/focal-updates,focal-security 390.151-0ubuntu0.20.04.1 amd64
nvidia-driver-390/focal-updates,focal-security 390.151-0ubuntu0.20.04.1 i386
nvidia-driver-418-server/focal-updates,focal-security 418.226.00-0ubuntu0.20.04.2 amd64
nvidia-driver-418/focal 430.50-0ubuntu3 amd64
nvidia-driver-430/unknown 515.48.07-0ubuntu1 amd64
nvidia-driver-435/focal-updates 455.45.01-0ubuntu0.20.04.1 amd64
nvidia-driver-440-server/focal-updates,focal-security 450.191.01-0ubuntu0.20.04.1 amd64
nvidia-driver-440/focal-updates,focal-security 450.119.03-0ubuntu0.20.04.1 amd64
nvidia-driver-450-server/focal-updates,focal-security 450.191.01-0ubuntu0.20.04.1 amd64
nvidia-driver-450/unknown 450.191.01-0ubuntu1 amd64
nvidia-driver-455/unknown 455.45.01-0ubuntu1 amd64
nvidia-driver-460-server/focal-updates,focal-security 470.129.06-0ubuntu0.20.04.1 amd64
nvidia-driver-460/unknown 460.106.00-0ubuntu1 amd64
nvidia-driver-465/unknown 465.19.01-0ubuntu1 amd64
nvidia-driver-470-server/focal-updates,focal-security 470.129.06-0ubuntu0.20.04.1 amd64
nvidia-driver-470/unknown 470.129.06-0ubuntu1 amd64
nvidia-driver-495/unknown 495.29.05-0ubuntu1 amd64
nvidia-driver-510-server/focal-updates,focal-security 510.73.08-0ubuntu0.20.04.1 amd64
nvidia-driver-510/unknown 510.73.08-0ubuntu1 amd64 [upgradable from: 510.73.05-0ubuntu0.20.04.1]
nvidia-driver-515-server/focal-updates,focal-security 515.48.07-0ubuntu0.20.04.1 amd64
nvidia-driver-515/unknown 515.48.07-0ubuntu1 amd64

# Install a driver (commented for safety):

# sudo apt install -y nvidia-driver-510
```

## Confirm setup - NVIDIA Docker

Follow the installation guide for NVIDIA docker container toolkit: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

Make sure nvidia docker is working by running `nvidia-smi` within the docker environment:

```bash
sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi
```

# Now run a deepstream example

```bash
# Run the docker:
docker run \
--gpus all \
-it \
--rm \
--net=host \
--privileged \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v $(pwd):/code/ \
-e DISPLAY=$DISPLAY \
-e CUDA_VER=11.6 \
-w /opt/nvidia/deepstream/deepstream-6.1/sources/apps/sample_apps/deepstream-test1 \
nvcr.io/nvidia/deepstream:6.1-devel

# Build the example (within the docker):
make

# Run the example (within the docker):
./deepstream-test1-app ../../../../samples/streams/sample_720p.h264

```



