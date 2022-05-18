# HyperBot
Main Repository for submission "HYPERBOT – ROBOTIC WORKCELL DESIGN FOR JOINT ACQUISITION OF SCENE AND GRASP SPECTRAL DATA" to WHISPERS 2022

Author: Nathaniel Hanson
Contact: hanson.n at northeastern dot edu

This repository is a collection of code meant to foster further collaboration and integration of hyperspectral imaging into robotics research and is continually being improved. These instructions are meant to be general and provide enough detail for someone familiar with Linux/Unix systems to get up and running. The best way to communicate issues with the code is through the `Issues` tab on the GitHub page. 

### Compute Requirements
The code in this repository was developed and tested on a desktop PC running Ubuntu 20. The code in this repository _may_ work in Windows 10, but is currently untested.

### Hardware List
#### Computer
- Desktop computer with > 32 GB of RAM
#### Networking
- 4 Port Ethernet [Router](https://www.amazon.com/gp/product/B0756QFLXP/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&th=1)
#### Robot
- Universal Robotics [UR3e CoRobot](https://shop.axisne.com/universal-robots-ur3e-robot/ecomm-product-detail/314069/)
- LOPRO [Linear Rail Actuator](https://www.bwc.com/products/actuated-linear-guide-systems/lopror-linear-actuators/lopro-07f7d7d24576d8a913150b422b6f6368.html)
- Nano Tech [ST6018D4506-B](https://us.nanotec.com/products/1336-sc6018-stepper-motor-nema-24) Stepper Motor
- Nano Tech [C5-E-1-09](https://en.nanotec.com/products/1764-1764-c5-e-controller-for-stepper-motors-bldc) Motor Controller
- RobotIQ [2F-85 Gripper](https://robotiq.com/products/2f85-140-adaptive-robot-gripper)
#### Spectral Sensing
- Headwall Photonics [Hyperspec Nano](https://www.headwallphotonics.com/products/hyperspectral-sensors) Hyperspectral Camera
- StellarNet [BlueWave](https://www.stellarnet.us/spectrometers/blue-wave-miniature-spectrometers/) Mini Spectrometer (350-1150 nm specification)
- 7-around-1 fiber low OH [fiber optic cable](https://www.stellarnet.us/spectrometers-accessories/fiber-optic-cables/) in Y-configuration (R600-8-VisNIR)
- Quartz Tungsten Halogen calibrated [light source](https://www.stellarnet.us/light-sources/visible-light-sources/)
- Scene [work lights](https://www.amazon.com/Designers-Edge-L860-Portable-250-Watt/dp/B002OLBAHU)
### Imaging
- DEPSTECH [USB Endoscope Camera](https://www.amazon.com/DEPSTECH-Ultra-Thin-Inspection-Semi-Rigid-Adpater-16-5ft/dp/B0836XWPJH/ref=sr_1_3?keywords=usb+endoscope+camera+with+light&qid=1652824249&sprefix=usb+en%2Caps%2C101&sr=8-3)
- Microsoft [Azure Kinetc](https://www.microsoft.com/en-us/d/azure-kinect-dk/8pp5vxmd9nhq?activetab=pivot:overviewtab)

### Hardware Setup

Follow the guidance provided in the manuscript to assemble to rail + robot configuration. Further advice available on request, but is highly dependent on laboratory workspace configuration.

Next, download the CAD files located [here](https://github.com/RIVeR-Lab/SpectroVision/blob/main/README.md). These files contain the CAD models needed to modify the fingers of the 2F-85 gripper to accommodate the spectral probe and endoscope. For the scope of this paper, the endoscope can be mounted but is unused.

### Software Installation

#### Setting Up the Robot
Follow the instructions [here](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/install_urcap_e_series.md) to setup your UR3e robot to accept commands from an external computer. Please also follow instructions from the manufacturer to install the URCap for the 2F-85 gripper.

#### Installing ROS
This project is built on the open source Robot Operating System (ROS) Noetic, Python3, and C++14. Appropriately guide or getting started with ROS can be found [here](http://wiki.ros.org/noetic/Installation).

ROS Noetic, Python3.8, should be installed system wide using `sudo`. You will also need the [catkin_tools package](https://catkin-tools.readthedocs.io/en/latest/installing.html).

#### Azure Kinect Setup
Follow the instruction provided [here](https://github.com/microsoft/Azure_Kinect_ROS_Driver) from the official Microsoft GitHub.

#### Spectrometer Setup

##### Install Pyevn

The spectrometer uses a compiled cpthon object which is only compatible with python 3.6.X Follow the instructions [Here](https://github.com/pyenv/pyenv), but do not modify your .bashrc file
```
pyenv install 3.6.7
```
Take the path for installed python3.6 binary and use it as the shebang for `stellarnetdriver.py`. It should look something like:
```
#!<<INSERT YOUR HOME DIRECTORY HERE>>/.pyenv/versions/3.6.7/bin/python
```
#### Workspace Setup
To get started, begin by creating a new ROS workspace:

```
mkdir -p catkin_ws/src
cd catkin_ws
catkin_create_workspace
```

In the `src` directory created by the command, `git clone` the following packages:

```
https://github.com/UniversalRobots/Universal_Robots_ROS_Driver
https://github.com/a-price/robotiq_arg85_description
https://github.com/davidfornas/pcl_manipulation
https://github.com/nLinkAS/fmauch_universal_robot
https://github.com/ros/kdl_parser
https://github.com/RIVeR-Lab/robo_rail_public
https://github.com/RIVeR-Lab/spectral_finger_public
https://github.com/RIVeR-Lab/spectral_start_public
https://github.com/RIVeR-Lab/cluttered_grasp_public
https://github.com/RIVeR-Lab/spectral_manip_public
https://github.com/tkelestemur/point_cloud_proc
```
One final repository is needed to operate the hyperspectral camera within ROS, but cannot be publicly open-sourced due to manufacturer restrictions. Contact the repository authors to obtain a copy of this code.

With the repositories cloned, the ROS workspace can be built with `catkin build`

### Running the Code
In every terminal you will need to run the command `source catkin_ws/devel/setup.bash` to include the requisite binaries in the search path. These commands assume that the hyperspectral pushbroom scans are being actively published to the `/hsi` topic. Please contact the authors for further details on how to complete this step.

Run the following commands in separate terminals
_Terminal 1_: `roslaunch spectral_start robot_start.launch` 

On your UR3e teach pendant, run the external control program you previously completed.
_Terminal 2_: `roscd spectral_start && sudo ./scripts/setup.bash`

This enables the spectrometer to open serial communication with the computer through its software development kit. Next run: `roslaunch spectral_start system_start.launch`

_Terminal 3_: `rosrun cluttered_grasp collect_hypercube.py` To collect and register an HSI scan with the RGBD scene image. After running the collection, start the service `rosrun cluttered_grasp associate_rgb_hsi.py` to generate the registered image.

_Terminal 4_: `rosrun spectral_finger_planner object_cluster_server` To start the tabletop clustering service. Various hyper-parameters can be tweaked [here](https://github.com/tkelestemur/point_cloud_proc/blob/master/config/default.yaml) that control the plane removal and point cloud segmentation.

_Teriminal 5_:`rosrun spectral_finger_planner ur3e_planner.py` To start the grasping routine to pick up and remove table objects from the scene.
