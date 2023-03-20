>[!NOTE]
> This is a note message.
[![ROS2 VERSION](https://img.shields.io/badge/ROS-ROS%202%20Humble-brightgreen)](http://docs.ros.org/en/humble/index.html)
&nbsp;
[![Ubuntu VERSION](https://img.shields.io/badge/Ubuntu-22.04-green)](https://ubuntu.com/)
&nbsp;
[![DEPENDENCIES](https://img.shields.io/badge/dependencies-mini__pupper__bsp-critical)](https://github.com/mangdangroboticsclub/mini_pupper_bsp)
&nbsp;
[![LICENSE](https://img.shields.io/badge/license-Apache--2.0-informational)](https://github.com/mangdangroboticsclub/mini_pupper_ros/blob/ros2/LICENSE)
&nbsp;
[![Twitter URL](https://img.shields.io/twitter/url?style=social&url=https%3A%2F%2Ftwitter.com%2FLeggedRobot)](https://twitter.com/LeggedRobot)


# Mini Pupper ROS 2

Mini Pupper is an open source 12-DOF quadruped robot,supporting ROS 2 (Robot Operating System) designed for educational purposes, research and experimentation.
With ROS 2, you can explore SLAM and Navigation functions with Mini Pupper. The controller of Mini Pupper's ROS packages is based on Stanford QuadrupedRobot project.  
This repository contains the ROS 2 packages for Mini Pupper.


## Features
- Support for ROS 2 Humble and Ubuntu 22.04 LTS
- Built-in ROS 2 Interface for Stanford QuadrupedRobot
- ROS2 SLAM and Navigation functions supported


## Installation

* To use Gazebo simulator, "1. PC Setup" is required.
* To control Mini Pupper, "2. Mini Pupper Setup" is required.
* To control Mini Pupper using visualize tools, "1. PC Setup" and "2. Mini Pupper Setup" is required.





### 1. PC Setup

PC Setup corresponds to PC (your desktop or laptop PC) for controlling Mini Pupper remotely or execute simulator.  
<details>
<summary>Click here for PC Setup</summary>

:::note
Do not apply these PC Setup commands to your Raspberry Pi on Mini Pupper.
Ubuntu 22.04 + ROS 2 Humble is required.
:::
#### 1.1 ROS 2 installation
Please follow the [installation document for ROS Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html) or use the [unofficial ROS 2 installation script](https://github.com/Tiryoh/ros2_setup_scripts_ubuntu).

#### 1.2 Download the Mini Pupper ROS 2 & dependencies packages
After ROS 2 installation, download the Mini Pupper ROS package in the workspace.
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/mangdangroboticsclub/mini_pupper_ros.git -b ros2
vcs import < mini_pupper_ros/.minipupper.repos --recursive
```

#### 1.3 Build and install all ROS packages
```bash
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
sudo apt-get install ros-humble-teleop-twist-keyboard
pip install transforms3d
colcon build --symlink-install
```
</details>



### 2. Mini Pupper Setup


Mini Pupper Setup corresponds to the Raspberry Pi on your Mini Pupper.  
<details>
<summary>Click here for Mini Pupper Setup</summary>
> **Note**
> Do not apply these Mini Pupper Setup commands to your PC.
> Ubuntu 22.04 + ROS 2 Humble is required.  


#### 2.1 mini_pupper_bsp installation
You should first install dependencies of servos, battery moniter and display screen.  
See [mini_pupper_bsp](https://github.com/mangdangroboticsclub/mini_pupper_bsp).

#### 2.2 ROS 2 installation
After installing the driver software, install ROS 2. ROS 2 Humble is required.  
Please follow the [installation document for ROS Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html) or use the [unofficial ROS 2 installation script](https://github.com/Tiryoh/ros2_setup_scripts_ubuntu).

#### 2.3  Download the Mini Pupper ROS 2 & dependencies packages
After ROS 2 installation, download the Mini Pupper ROS package in the workspace.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/mangdangroboticsclub/mini_pupper_ros.git -b ros2
vcs import < mini_pupper_ros/.minipupper.repos --recursive
# compiling gazebo and cartographer on Raspberry Pi is not recommended
touch mini_pupper_ros/mini_pupper_gazebo/AMENT_IGNORE
touch mini_pupper_ros/mini_pupper_navigation/AMENT_IGNORE
```
#### 2.4 Build and install all ROS packages


```bash
# install dependencies without unused heavy packages
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y --skip-keys=joint_state_publisher_gui --skip-keys=rviz2 --skip-keys=gazebo_plugins --skip-keys=velodyne_gazebo_plugins
sudo apt-get install ros-humble-teleop-twist-keyboard
pip install transforms3d
colcon build --symlink-install
```
:::tip
If the Raspberry Pi has less than 4GB memory
try `MAKEFLAGS=-j1 colcon build --executor sequential --symlink-install` 
instead of `colcon build --symlink-install`
:::

</details>


## Usage
To try the Mini Pupper ROS2, follow these steps to run the examples.



### 1. PC
**These steps are only for PC.**
<details>
<summary>Click here for examples on PC</summary>


#### 1.1 Test in Rviz2
View Mini Pupper model in Rviz2 and control it with keyboard.
```bash
# Terminal 1
. ~/ros2_ws/install/setup.bash # setup.zsh if you use zsh instead of bash
ros2 launch mini_pupper_bringup bringup.launch.py joint_hardware_connected:=false rviz:=true

# Terminal 2
ros2 run teleop_twist_keyboard teleop_twist_keyboard
# Then control robot dog with the keyboard
```



#### 1.2 Test in Gazebo
Control the mini pupper in Gazebo with keyboard.
```bash
# Terminal 1
. ~/ros2_ws/install/setup.bash # setup.zsh if you use zsh instead of bash
ros2 launch mini_pupper_gazebo gazebo.launch.py rviz:=true

# Terminal 2
ros2 run teleop_twist_keyboard teleop_twist_keyboard
# Then control robot dog with the keyboard
```

#### 1.3  Test Cartographer in Gazebo
Try SLAM in Gazebo with keyboard.

```bash
# Terminal 1
. ~/ros2_ws/install/setup.bash
ros2 launch mini_pupper_gazebo gazebo.launch.py

# Terminal 2
. ~/ros2_ws/install/setup.bash
ros2 launch mini_pupper_navigation slam.launch.py use_sim_time:=true

# Terminal 3
ros2 run teleop_twist_keyboard teleop_twist_keyboard
# Then control robot dog with your keyboard
```
:::tip
If you want to save the map:
```bash
ros2 service call /finish_trajectory cartographer_ros_msgs/srv/FinishTrajectory "{trajectory_id: 0}"
ros2 service call /write_state cartographer_ros_msgs/srv/WriteState "{filename: '${HOME}/mymap.pbstream'}"
ros2 run nav2_map_server map_saver_cli -f ${HOME}/mymap
```
:::


</details>

### 2. Mini Pupper
**These steps are only for real Mini Pupper.**
<details>
<summary>Click here for examples on real Mini Pupper</summary>

#### 2.1 Test walk

Open 2 terminals and ssh login to Mini Pupper on both.

```sh
# Terminal 1 (ssh)
. ~/ros2_ws/install/setup.bash # setup.zsh if you use zsh instead of bash
ros2 launch mini_pupper_bringup bringup.launch.py

# Terminal 2 (ssh)
ros2 run teleop_twist_keyboard teleop_twist_keyboard
# Then control Mini Pupper with the keyboard
```
</details>

## FAQ

* Q. Is Ubuntu 20.04 supported?
  * A. No. Ubuntu 22.04 only for now.
* Q. Is ROS 2 Foxy/Galactic supported?
  * A. No. ROS 2 Humble only for now.
* Q. `colcon build` shows `1 package had stderr output: mini_pupper_control`.
  * A. The following warnings can be safely ignored. See [mini_pupper_ros#45](https://github.com/mangdangroboticsclub/mini_pupper_ros/pull/45#discussion_r1104759104) for details.
  ```bash
  Starting >>> mini_pupper_description
  --- stderr: mini_pupper_control
  /usr/lib/python3/dist-packages/setuptools/command/easy_install.py:158: EasyInstallDeprecationWarning: easy_install command is deprecated. Use build and pip and other standards-based tools.
    warnings.warn(
  /usr/lib/python3/dist-packages/setuptools/command/install.py:34: SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.
    warnings.warn(
  /usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning: 1.1build1 is an invalid version and will not be supported in a future release
    warnings.warn(
  /usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning: 0.1.43ubuntu1 is an invalid version and will not be supported in a future release
    warnings.warn(
  ---
  Finished <<< mini_pupper_control [7.37s]
  ```
  
  

  
## Contributing
Contributions are welcome! Please read the [contributing guidelines](CONTRIBUTING.md) before submitting a pull request.



## License

```
Copyright 2022-2023 MangDang

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

## Acknowledgments 

[Stanford QuadrupedRobot](https://github.com/mangdangroboticsclub/QuadrupedRobot)

