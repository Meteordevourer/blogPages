---
title: ros problems
date: 2021-04-10 17:46:25
categories:
- ros&毕设
tags:
- ros
- problems
---
## URDF问题
### [joint_state_publisher-2] process has died
![jpg](../rosproblems/processdied.png)
问题原因: URDF文件中第一行不能含有注释，整个文件不能有中文注释
解决方案：删除注释就好啦
### Could not find the GUI, install the 'joint_state_publisher_gui' package
![jpg](../rosproblems/joint_state_publisher_gui.png)
问题原因：joint_state_publisher_gui是刚更新出来的包，需要把之前的joint_state_publisher换成joint_state_publisher_gui
解决方法：
1. 输入下面的命令
> sudo apt-get install ros-melodic-joint-state-publisher-gui
 
注意ros版本区别，我这里是melodic，kinetic版记得更改
2. 如果还不可以
把launch文件中的joint_state_publisher用joint_state_publisher_gui全部替换
