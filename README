# Mini Pupper在ROS中的仿真
---
## 课程概述 
在这个课程中，你将会学习Mini Pupper结合Rivz和Gazebo的实例应用。你将使用Rviz可视化机器人模型，并在Gazebo仿真环境中实现Mini Pupper的键盘移动控制，然后带着Mini Pupper在你搭建的虚拟世界中散步。

---
## 关于课程 
按照规划，本课程需要花费大约2小时来完成，目标受众为四足机器人开发爱好者。
**note**
本课程是基于[Mangdang](https://www.mangdang.net/)的Mini Pupper系列产品搭建的。

---
## 学习目标
在这个课程中，你将会学到：
 - 如何使用可视化工具Rviz来观察Mini Pupper机器人模型
 - 如何使用仿真工具Gazebo在虚拟环境中控制Mini Pupper
 - 如何使用并监视一个Mini Pupper键盘控制ROS节点



 ---
## 课程细节
本课程将使用以下组件：
 - [Ubuntu](https://ubuntu.com/)是一个以桌面应用为主的Linux操作系统，具有良好的版本维护和社区环境
 - [ROS](https://www.ros.org/)是一个机器人通用的开发的平台，可帮助您方便地构建机器人应用程序
 - [Rviz](http://wiki.ros.org/rviz/UserGuide)是ROS体系下的 3D 数据可视化工具
 - [Gazebo](https://gazebosim.org/home)是3D仿真工具，它能配合ROS组成一个强大的机器人仿真测试环境

---
## 准备材料
在开始这个课程前，你需要具备以下知识：
 - **对 Linux 操作系统的基本了解**
 - **对ROS的基本了解**
 - **对Rviz和Gazebo的基本了解**

在开始这个课程前，你需要准备好以下材料：
 - **安装有Ubuntu22.04系统并配置好ROS2 Humble环境的笔记本电脑**
 - **能够访问Github等网站的良好网络状态**


---

# 引言
四足机器人仿真可以快速、低成本、高安全性地验证四足机器人的结构设计、运动控制等工作的原理有效性。通过仿真结果与实际测试结果的对比，可以帮助机器人开发者更好地完善机器人。

在本课程中，仿真意味着你无需理会Mini Pupper实际硬件的组装和调试，就能快速上手四足机器人并在仿真环境中找到如同遥控玩具车一样简单的乐趣。

---



## 整体步骤
1. [检查Ubuntu22.04和ROS2 Humble的环境配置](#step1)
1. [查看并熟悉Rviz的窗口和组件](#step2)
1. [部署四足机器人数据可视化与仿真环境](#step3)
1. [了解URDF](#step4)
1. [在Rviz中观察机器人模型](#step5)
1. [配置键盘控制节点](#step6)
1. [通过键盘控制Rviz中的Mini Pupper](#step7)
1. [查看并熟悉Gazebo的窗口和组件](#step8)
1. [为Mini Pupper构建仿真世界](#step9)
1. [调整Gazebo环境](#step10)
1. [带着Mini Pupper在你构建的虚拟世界中散步](#step11)

---
# 任务1：键盘控制Rviz中的Mini Pupper模型
Rviz是ROS中非常受欢迎的数据可视化工具，通过将机器人模型导入Rviz，并发布机器人控制信息，你将能通过键盘控制Rviz中的Mini Pupper运动。

![在这里插入图片描述](https://img-blog.csdnimg.cn/7287c5360b494fad90cf9c1199acc99e.gif)


---
<span id="step1"></span>
## 第一步：检查Ubuntu22.04和ROS2 Humble的环境配置
Ubuntu22.04和ROS2 Humble的环境是搭建mini pupper仿真环境的前提，请先检查你是否完成了课程的准备材料。
1. 查看系统内核版本号及系统名称
确认系统版本是否为Ubuntu22.04
```bash
uname -a
```
![](https://img-blog.csdnimg.cn/ddf64adb535240f78a1db1c167aafa28.png)

2. 查看ROS版本
确认ROS版本是否为Humble
```bash
printenv ROS_DISTRO
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2a27890c51084452aae7ecc19043a1fa.png)

---

<span id="step2"></span>

## 第二步：查看并熟悉Rviz的窗口和组件
1. 打开Rviz并观察整体界面布局
ROS2 Humble版本中使用的Rviz版本为Rviz2

```bash
rviz2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b93fda15bff442adbab087b121ffe0be.png)

2. 观察左侧状态栏目Displays
Displays部分是在三维空间中绘制图像的组件，通过选择订阅话题等方式，能将机器人的数据可视化。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e711656e4dd4ace87f3048826ab4041.png)
Rviz支持许多常见的传感器的数据，比如激光雷达、相机等。
通过Rviz插件，甚至可以将特定的冷门传感器数据可视化。
![在这里插入图片描述](https://img-blog.csdnimg.cn/47c45fd7eb5149e1942eb2d6fbaef716.png)
3. 观察右侧状态栏目Views和中间的界面
在Views栏目中，你可以调整你的观察视角。
默认的相机模式为轨道相机，当移动相机时，中间界面出现的黄色圆盘即是相机的焦点。
![](https://img-blog.csdnimg.cn/2c18b313bfbb40aea5c90c34695bc2a0.png)
你可以通过这些按键调整视角：
 - 鼠标左键：长按左键拖动旋转视野
 - 鼠标中键：移动焦点位置
 - 鼠标右键：放大/缩小视野
4. 观察上侧的导航部分
通过和导航包Navigation的配合，在Rviz中，只需要点一下你的鼠标，使用Rviz上侧的几个按钮，就可以进行导航。
 - 2D Pose Estimate：为机器人选择初始位置和姿态
 - 2D Goal Pose：向"goal"话题发送目标位置信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c01b7b14d674b29b148079aa7ef7e2f.png)

---

<span id="step3"></span>

## 第三步：部署四足机器人数据可视化与仿真环境
 1. 在工作区下载Mini Pupper ROS包。
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/mangdangroboticsclub/mini_pupper_ros.git -b ros2
```
2. 使用[vcstool](https://github.com/dirk-thomas/vcstool)版本控制工具解决Python包版本依赖问题。

```bash
vcs import < mini_pupper_ros/.minipupper.repos --recursive
```
**note**
如果你未安装vcstool，请先安装vcstool。

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install python3-vcstool
```
3. 构建并安装需要的 ROS 包
回到ros2_ws工作空间下，解决ROS包依赖问题，安装需要的键盘控制ROS包并构建。
```bash
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y  # solve the dependency of ros package
sudo apt-get install ros-humble-teleop-twist-keyboard  # keyboard package
colcon build --symlink-install  # build
```
**note**
如果你遇到rosdep相关问题，请查看[背景信息和资源](#backinfo)

4. 预下载Gazebo常见模型
在首次启动Gazebo时会耗费很长时间准备模型文件，需要预先下载模型以避免耗时。
```bash
cd ~/.gazebo/
git clone https://github.com/osrf/gazebo_models.git models
```
 
 ---
 <span id="step4"></span>

## 第四步：了解URDF
Unified Robot Description Format(URDF)是统一机器人描述格式，它是一种用于表示机器人模型的 XML 格式，能对机器人进行描述。

1. 打开并查看URDF文件
```bash
cd ~/ros2_ws/src/mini_pupper_ros/mini_pupper_description/urdf
sudo apt install gedit
sudo gedit mini_pupper_description.urdf
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb02e47e43b84ae0addcee09d5acb498.png)

2. 将urdf文件转化为图像
urdf文件通常难以阅读，将urdf文件转化为树状结构图，了解机器人的模型描述。
```bash
cd ~/ros2_ws/src/mini_pupper_ros/mini_pupper_description/urdf
urdf_to_graphviz mini_pupper_description.urdf  # translate urdf to tree graph
xdg-open mini-pupper.pdf  # open graph pdf
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a9c1563308447409e5924182512a71f.png)
**note**
如果你希望深入了解URDF和Xacro，请查看[背景信息和资源](#backinfo)

--- 
<span id="step5"></span>
## 第五步：在Rviz中观察机器人模型
在Rviz中，机器人模型可作为Displays的对象，Displays将以正确的位姿显示机器人可视化模型，并且这个模型受当前的TF transforms定义。
1. 启动Mini Pupper的节点打开Rviz观察机器人模型的可视化信息。

```bash
. ~/ros2_ws/install/setup.bash # setup.zsh if you use zsh instead of bash
ros2 launch mini_pupper_bringup bringup.launch.py joint_hardware_connected:=false rviz:=true
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/338454d8ac214b89a50f8b598ec61309.png)
2. 查看/robot_description话题
经过此前启动文件的配置，robot_state_publisher节点将发布机器人的描述信息，Rviz将接收robot_description话题信息并更新机器人模型。

```bash
ros2 topic info -v /robot_description
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/503becfeb4874bd7aec0e76dba428b28.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e50074e5e06e40b5b1ae62a3689ff3fc.png)



**note**
如果你希望深入了解TF2，请查看[背景信息和资源](#backinfo)

---
<span id="step6"></span>
## 第六步：配置键盘控制节点
1. 打开键盘控制节点
通过键盘可发布/cmd_vel话题，控制机器人旋转、移动。
键盘控制方式如下，按住shift可以切换到平移模式。
$$
\left[
\begin{matrix}
	u & i & o \\
	j & k & l \\
	m & , & . \\
\end{matrix}
\right]
=
\left[
\begin{matrix}
	\nwarrow & \uparrow & \nearrow \\
	\leftarrow & stop & \rightarrow \\
	\swarrow & \downarrow & \searrow\\
\end{matrix}
\right]
$$




```bash
gnome-terminal --title "keyboard" -- ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6229d0f9d54a4071a6414df5e8b7d522.png)

2. 查看当前正在发布的话题
teleop_twist_keyboard发布/cmd_vel话题，quadruped_controller_node节点订阅/cmd_vel话题以控制机器人的运动。

```bash
gnome-terminal  -- bash -c "ros2 topic info -v /cmd_vel; exec bash"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e32a1097bb844af7ba1a0332ac495908.png)

3. 查看当前正在发布的话题内容
键盘节点正在以geometry_msgs/msg/Twist的数据格式发布状态信息。

```bash
gnome-terminal  -- bash -c "ros2 topic echo /cmd_vel; exec bash"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e091ed5bc60d40fba8c99b13dd96976b.png)
**note**
如果你希望深入了解ROS话题机制，请查看[背景信息和资源](#backinfo)

---
<span id="step7"></span>
## 第七步：通过键盘控制Rviz中的Mini Pupper
通过键盘终端输入移动指令，观察Rviz中Mini Pupper的运动。
![在这里插入图片描述](https://img-blog.csdnimg.cn/96f4c2549f3545b2a36e03ef273980e9.gif)



---

# 任务2：键盘控制Mini Pupper在Gazebo仿真里散步
Gazebo是强大的3D仿真工具，它能配合ROS组成一个强大的机器人仿真测试环境。
通过自己构建一个Gazebo虚拟世界并配合机器人模型，你将能通过键盘控制Gazebo虚拟世界中的Mini Pupper运动。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9214092b95604229ba2adf6707e515e5.gif)

---

 <span id="step8"></span>

## 第八步： 查看并熟悉Gazebo的窗口和组件
1. 打开gazebo并观察整体界面布局
当不添加参数地打开gazebo时，将会打开一个空世界，这将方便你对gazebo进行观察。
```bash
gazebo
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f88459b1c9b140b2a1643a2821283929.png)

**note**
在打开gazebo前，请务必先按照[第三步：部署四足机器人数据可视化与仿真环境](#step3)下载好模型。


2. 熟悉左侧工作栏
左侧的工作栏包含World里的各个实体模型、光照等环境属性、物理属性的设置。
其中的Insert部分允许你向世界添加之前预下载的各类复杂模型，比如房子、巴士、可乐瓶。
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8d5d483ef114a3ca113dd25e66e2885.png)
3. 熟悉调整观察角度和旋转平移模型

在Gazebo中，你可以通过这些按键调整视角：
 - 鼠标左键：平移视野
 - 鼠标中键：旋转视野
 - 鼠标右键：放大/缩小视野

![在这里插入图片描述](https://img-blog.csdnimg.cn/eb5b8f35dc074b328281d7bf1ab96417.png)
在Gazebo的上方工具栏中，你可以使用这几个按钮来平移、旋转调整模型。
如下是将一个实体模型旋转朝向，按住Shift可以帮助你更精确地调整。
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ff45095df9d4f81a55a00b83c4f71a9.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4924a435cb37431e866f9383eb76f170.png)


4. 在gazebo世界中尝试放置物体
点击Gazebo上方的简单几何体按钮可以将实体放置在世界中。
![在这里插入图片描述](https://img-blog.csdnimg.cn/d12a509168f341b3ad3b8aedc8c62e43.png)

除了能放置简单的几何物体，通过左侧栏目的Insert选项添加模型，你甚至能放置复杂的公共汽车，这得益于你此前预下载的模型库。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1537ecb1845f463e92feb3e6b7eb18b8.png)

---

<span id="step9"></span>
## 第九步： 为Mini Pupper构建仿真世界
你可以为Mini Pupper构建一个你喜欢的仿真环境。
通过使用简单的几何模型、预下载的复杂模型、旋转与平移，你能获取一个良好的仿真环境。
下图为使用了salon、suv、school、tablemarble的简单世界。
1. 搭建仿真世界
![在这里插入图片描述](https://img-blog.csdnimg.cn/bfbe0b8264154b7f9d0879c2eae74801.jpeg)

--- 
<span id="step10"></span>
## 第十步： 调整Gazebo环境
1. 保存仿真世界
 - 点击左上角的File按钮
 - 点击save world as
 - 将世界命名为**playground.world**
 
 2. 将自拟的世界文件移动到指定目录下
Mini Pupper Gazebo的demo world默认保存在该目录下
```bash
 ~/ros2_ws/src/mini_pupper_ros/mini_pupper_gazebo/world
```
将自拟的世界文件**playground.world**替换原有的世界文件**playground.world**

---

<span id="step11"></span>
## 第十一步：带着Mini Pupper在你构建的虚拟世界中散步
现在，你可以带着Mini Pupper在你自拟的虚拟世界中散步了！

1. 启动Mini Pupper Gazebo Demo节点
```bash
. ~/ros2_ws/install/setup.bash # setup.zsh if you use zsh instead of bash
ros2 launch mini_pupper_gazebo gazebo.launch.py rviz:=true
```
2. 启动键盘控制节点
在新的终端窗口下，启动键盘节点

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/49c6a9fcbd164be097efca87b55e77bf.gif)

---


# 总结Summary

经过本课程的学习，你应该能达到以下水平：
| 知识点 | 内容 | 了解|熟悉|掌握|
|--|--|--|--|--|
Rviz|使用可视化工具Rviz来观察机器人模型||&#10004;||
Gazebo|搭建世界||&#10004;||
Gazebo|使用仿真工具Gazebo在虚拟环境中控制机器人|&#10004;|||
ROS|使用并监视一个ROS节点||&#10004;||

--- 

<span id="backinfo"></span>
# 背景信息和资源Background Information and Resources
      
[Reference : ROS2 Humble rosdep](https://docs.ros.org/en/humble/Tutorials/Intermediate/Rosdep.html)

[Reference : setup.bash for configuring environment](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)

[Reference : Rviz in ROS Wiki](http://wiki.ros.org/rviz/UserGuide)

[Reference : URDF](http://docs.ros.org/en/humble/Tutorials/Intermediate/URDF/URDF-Main.html#urdf)

[Reference : Xacro](http://docs.ros.org/en/humble/Tutorials/Intermediate/URDF/Using-Xacro-to-Clean-Up-a-URDF-File.html)

[Reference : vcstool](https://github.com/dirk-thomas/vcstool)

[Reference : TF2](http://docs.ros.org/en/humble/Tutorials/Intermediate/Tf2/Tf2-Main.html#tf2)

[Reference : ROS Topic](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)

**版权信息：教材尚未完善，此处预留版权信息处理方式**
mini pupper相关内容可访问：[https://github.com/mangdangroboticsclub](https://github.com/mangdangroboticsclub)
