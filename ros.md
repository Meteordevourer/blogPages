---
title: 机器人系统设计
date: 2021-04-09 14:21:59
categories:
- ros&毕设
tags:
- ros
---
  写在前面
  由于本blog诞生是在进行了一部分ros工作之后，例如ros环境搭建，消息监听，动作编程等均以完成，本篇就直接进入机器人系统设计环节。~~如果以后有闲功夫就将前面的补上~~
> keywords:机器人的定义与组成 机器人系统构建 URDF机器人建模
## 机器人的定义与组成
{% blockquote %}
打破固定思维，机器人不一定是人形的！应该与应用环境有关
{% endblockquote %}
下图是一张机器人组成的说明图
![robot](../ros/componentsofrobot.png)
## 机器人系统构建
### 执行机构的实现
![perform](../ros/perform.png)
### 驱动系统的实现
![perform](../ros/driving.png)
### 内部传感器实现
![perform](../ros/sensors.png)
![perform](../ros/IMUsensor.png)
### 控制系统实现
![perform](../ros/control.png)
### 外部传感器连接
#### 连接激光雷达
首先，安装所需要的功能包
{% blockquote %}
sudo apt-get install ros-melodic-rplidar-ros
rosrun rplidar_ros rplidarNode
{% endblockquote %}
![perform](../ros/radarconnect.png)
## URDF建模
### 什么是URDF？
{% blockquote %}
Unified Robot Description Format——统一机器人描述格式
ROS中一个非常重要的机器人模型描述格式
可以解析URDF文件中使用XML格式描述的机器人模型
ROS同时也提供URDF文件的C++解析器
{% endblockquote %}
URDF通过多个link和joint来描述机器人的结构
#### link
> 描述机器人某个刚体部分的外观和物理属性
> 尺寸、颜色、形状、惯性矩阵、碰撞参数等
* visual:描述机器人link部分的外观参数
* inertial:描述link的惯性参数
* collision:描述link的碰撞属性
![perform](../ros/linkparam.png)
#### joint
> 描述机器人关节的运动学和动力学属性
> 包括关节运动的位置和速度限制
> 根据关节运动形式，可以分为6种类型
> ![perform](../ros/jointtypes.png)
* calibration:关节的参考位置，用来校准关节
#### robot
> 完整的机器人模型的最顶层标签
> link和joint标签必须都包含在robot标签内
![perform](../ros/robottag.png)

那么就让我们现在开始进行一次简单的URDF建模吧！
### URDF操作流程
创建一个机器人建模的功能包
> 在ros工作空间的src文件夹下打开终端
> 输入命令catkin_create_pkg mbot_description urdf xacro
![perform](../ros/urdfcreate.png)

urdf描述文件命名方式常用name_description，添加urdf依赖(解析模型)和xacro依赖
之后在创建的模型文件夹内创建四个文件夹，分别是
* urdf:存放机器人模型的URDF或xacro文件
* meshes:放置机器人外观纹理描述文件，urdf中引用的模型渲染文件
* launch:保存相关启动文件
* config:保存rviz的配置文件
然后，在launch文件夹下创建一个launch文件
![perform](../ros/urdflaunch.png)
{% fold %}
{% codeblock lang:HTML %}

<robot name="mbot">

    <link name="base_link">
        <visual>
            <origin xyz=" 0 0 0" rpy="0 0 0" />
            <geometry>
                <cylinder length="0.16" radius="0.20"/>
            </geometry>
            <material name="yellow">
                <color rgba="1 0.4 0 1"/>
            </material>
        </visual>
    </link>

    <joint name="left_wheel_joint" type="continuous">
        <origin xyz="0 0.19 -0.05" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="left_wheel_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="left_wheel_link">
        <visual>
            <origin xyz="0 0 0" rpy="1.5707 0 0" />
            <geometry>
                <cylinder radius="0.06" length = "0.025"/>
            </geometry>
            <material name="white">
                <color rgba="1 1 1 0.9"/>
            </material>
        </visual>
    </link>
    <joint name="right_wheel_joint" type="continuous">
        <origin xyz="0 -0.19 -0.05" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="right_wheel_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="right_wheel_link">
        <visual>
            <origin xyz="0 0 0" rpy="1.5707 0 0" />
            <geometry>
                <cylinder radius="0.06" length = "0.025"/>
            </geometry>
            <material name="white">
                <color rgba="1 1 1 0.9"/>
            </material>
        </visual>
    </link>
    <joint name="front_caster_joint" type="continuous">
        <origin xyz="0.18 0 -0.095" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="front_caster_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="front_caster_link">
        <visual>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <geometry>
                <sphere radius="0.015" />
            </geometry>
            <material name="black">
                <color rgba="0 0 0 0.95"/>
            </material>
        </visual>
    </link>

    <joint name="back_caster_joint" type="continuous">
        <origin xyz="-0.18 0 -0.095" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="back_caster_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="back_caster_link">
        <visual>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <geometry>
                <sphere radius="0.015" />
            </geometry>
            <material name="black">
                <color rgba="0 0 0 0.95"/>
            </material>
        </visual>
    </link>

    <link name="camera_link">
        <visual>
            <origin xyz=" 0 0 0 " rpy="0 0 0" />
            <geometry>
                <box size="0.03 0.04 0.04" />
            </geometry>
            <material name="black">
                <color rgba="0 0 0 0.95"/>
            </material>
        </visual>
    </link>

    <joint name="camera_joint" type="fixed">
        <origin xyz="0.17 0 0.10" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="camera_link"/>
    </joint>

    <link name="kinect_link">
        <visual>
            <origin xyz="0 0 0" rpy="0 0 -1.5708"/>
            <geometry>
                <mesh filename="package://mbot_description/meshes/kinect.dae" />
            </geometry>
        </visual>
    </link>

    <joint name="kinect_joint" type="fixed">
        <origin xyz="-0.15 0 0.11" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="kinect_link"/>
    </joint>

    <link name="laser_link">
		<visual>
			<origin xyz=" 0 0 0 " rpy="0 0 0" />
			<geometry>
				<cylinder length="0.05" radius="0.05"/>
			</geometry>
			<material name="black"/>
		</visual>
    </link>

    <joint name="laser_joint" type="fixed">
        <origin xyz="0 0 0.105" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="laser_link"/>
    </joint>
</robot>
{% endcodeblock %}
{% endfold %}

