A more detailed unitree_guide document is available at unitree [developer site](https://support.unitree.com/home/zh/Algorithm_Practice).( only Chinese version now )

# Overview

The unitree_guide is an open source project for controlling the quadruped robot of Unitree Robotics, and it is also the software project accompanying [《四足机器人控制算法--建模、控制与实践》](https://detail.tmall.com/item.htm?spm=a212k0.12153887.0.0.5487687dBgiovR&id=704510718152) published by Unitree Robotics.

# Quick Start
The following will quickly introduce the use of unitree_guide in the gazebo simulator. For more usage, please refer to 《四足机器人控制算法--建模、控制与实践》.
## Environment
We recommand users to run this project in Ubuntu 18.04 and ROS melodic environment.
## Dependencies
1. [unitree_guide](https://github.com/unitreerobotics/unitree_guide)<br>
2. [unitree_ros](https://github.com/unitreerobotics/unitree_ros)<br>
3. [unitree_legged_msgs](https://github.com/unitreerobotics/unitree_ros_to_real)(Note that: unitree_legged_real package should not be a part of dependencies)<br>

Put these three packages in the src folder of a ROS workspace.

## build
Open a terminal and switch the directory to the ros workspace containing unitree_guide,  then run the following command to build the project:
```
catkin_make
```
If you have any error in this step, you can raise an issue to us.
## run
In the same terminal, run the following command step by step:
```
source ./devel/setup.bash
```
To open the gazebo simulator, run:
```
roslaunch unitree_guide gazeboSim.launch 
```

For starting the controller, open an another terminal and switch to the same directory,  then run the following command:
```
./devel/lib/unitree_guide/junior_ctrl
```

## usage
After starting the controller,  the robot will lie on the ground of the simulator, then press the '2' key on the keyboard to switch the robot's finite state machine (FSM) from **Passive**(initial state) to **FixedStand**,  then press the '4' key to switch the FSM from **FixedStand** to **Trotting**, now you can press the 'w' 'a' 's' 'd' key to control the translation of the robot, and press the 'j' 'l' key to control the rotation of the robot. Press the Spacebar, the robot will stop and stand on the ground
. (If there is no response, you need to click on the terminal opened for starting the controller and then repeat the previous operation)

# Note
Unitree_guide provides a basic quadruped robot controller for beginners. To achive better performance, additional fine tuning of parameters or more advanced methods (such as MPC etc.) might be required. Any contribution and good idea from the robotics community are all welcome. Feel free to raise an issue ~ <br>

---

# Addition information

## Environment setup

The simulator(gazebo) is run with nvidia GPU, so please make sure GPU is correctly configured if you use docker settings directly. Below are the steps to configure GPU:

### Install CUDA

Follow the steps in the [link](https://developer.nvidia.com/cuda-downloads) to install CUDA, the drivers
will be installed when you install CUDA.

After CUDA is installed, run following commands to check installation:

```bash
$ nvidia-smi

# Test output
Sun Aug 31 13:05:48 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.65.06              Driver Version: 580.65.06      CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4050 ...    On  |   00000000:01:00.0  On |                  N/A |
| N/A   46C    P8              2W /   80W |     366MiB /   6141MiB |      4%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A           27268      G   /usr/lib/xorg/Xorg                      136MiB |
|    0   N/A  N/A           27541      G   /usr/bin/gnome-shell                     32MiB |
|    0   N/A  N/A           28162      G   ...exec/xdg-desktop-portal-gnome          2MiB |
|    0   N/A  N/A           37088      G   ...ersion=20250829-130006.660000         43MiB |
|    0   N/A  N/A           78545      G   /usr/share/code/code                     95MiB |
+-----------------------------------------------------------------------------------------+
```

```bash
nvcc --version

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Wed_Jan_15_19:20:09_PST_2025
Cuda compilation tools, release 12.8, V12.8.61
Build cuda_12.8.r12.8/compiler.35404655_0
```

### Install Nvidia container toolkit

Follow the steps in the [link](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) to install container toolkit.

After toolkit is installed, open repository with vs code, rebuild the container and run following commands to test installation:

```bash
$ nvidia-smi

# It should generate similar result as above step
```

```bash
$ glxinfo -B

# The vendor should be "Nvidia ..."
```

If you find out Nvidia GPU is not needed, then disable these lines in Docker file and rebuild container.

```
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=all
```