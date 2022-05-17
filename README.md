# HyperBot
WHISPERS 2022 - Main Repository for submission "HYPERBOT – ROBOTIC WORKCELL DESIGN FOR JOINT ACQUISITION OF SCENE AND GRASP SPECTRAL DATA"

This repository is in the process of being updated - please be patient as links are fixed to most up to date code.
### Compute Requirements
The code in this repository was developed and tested on a desktop PC running Ubuntu 20. The code in this repository _may_ work in Windows 10, but is currently untested

### Hardware List
#### Computer
- Desktop computer with > 32 GB of RAM

#### Networking
- 4 Port Ethernet Router
#### Robot
- Universal Robotics UR3e CoRobot
- LOPRO linear rail
- RobotIQ 2F-85 Gripper
#### Spectral Sensing
- Headwall Photonics Hyperspec Nano Hyperspectral Camera
- StellarNet BlueWave Mini Spectrometer (350-1150 nm specification)
- 7 fiber low OH fiber optic cable in Y-configuration
- Quartz Tungsten Halogen Light

### Software Installation
This project is built on the open source Robot Operating System (ROS) Noetic, Python3, and C++14. Appropriately guides for getting started with ROS can be found [here](http://wiki.ros.org/noetic/Installation).

To get started, begin by creating a new ROS workspace with `catkin_create_workspace`. In the `src` directory created by the command, `git clone` the following packages:

```
https://github.com/RIVeR-Lab/robo_rail_public
https://github.com/RIVeR-Lab/spectral_finger_public
https://github.com/RIVeR-Lab/spectral_start_public
https://github.com/RIVeR-Lab/cluttered_grasp_public
https://github.com/tkelestemur/point_cloud_proc
```
One final repository is needed to operate the hyperspectral camera within ROS, but cannot be publicly open-sourced due to manufacturer restrictions. Contact the authors to obtain a copy of this code.
