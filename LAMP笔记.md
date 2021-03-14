## 题目：

​	在地下感知退化的环境中 的探索   大尺度自动建图和定位 



## 摘要：

#### 	阐述已有的问题

现有存在问题：----（感知退化）

 1.照明不良，灰尘，水坑。使得仅仅依靠视觉方案不可靠

 2.路面湿滑，不平，使得车轮里程计不可靠。

 3.太大且特征不明显的环境使得 LiDAR建图产生漂移。



#### 	提出本文的贡献-----三点。

  1.We present a system architecture to enhance subterranean operation, including an accurate lidarbased front-end, and a flexible and robust back-end that automatically rejects outlying loop closures. 

  2.We present an extensive evaluation in large-scale, challenging subterranean environments, including the results obtained in the Tunnel Circuit of the DARPA Subterranean Challenge.

3. we discuss potential improvements, limitations of the state of the art, and future research directions.



使用slam的原因之一： GPS不能用。

激光slam在非结构化环境 中的使用比较多且有效抵抗光照的问题。



## introduction

## LAMP算法的阐述

### 单机器人和多机器人的架构

![image-20210224093237500](C:\Users\farlab\AppData\Roaming\Typora\typora-user-images\image-20210224093237500.png)

### 激光前端做里程估计 和回环检测。 使用LOAM

​		调用PCL库 对原始传感器数据做体素的过滤处理。

​		使用GICP的算法进行匹配。

![image-20210224112743760](C:\Users\farlab\AppData\Roaming\Typora\typora-user-images\image-20210224112743760.png)

### 视觉前端做人造标志的检测和定位

​		使用YOLO检测人造标志。产生2D检测框。

​		使用AprilTags library [30], [31], [32] to detect the fiducial markers. 

​		使用RGBD相机获得深度信息。

### 局部后端做PGO位姿图优化，估计机器人的轨迹。

​		在GTSAM中实现（暂时不知道是什么东西）：To compute the trajectory estimate, we optimize the pose graph using the Gauss-Newton implementation in GTSAM.

​		使用RVIZ和ROS进行试验，地图交互。

​		



### 基于ICM的拒绝策略

（在PGO后端之前，排除异常点），是对PCM算法进行修改，能够实现在线的操作。

位姿都是相对于机器人本体identity。



步骤：

​	1.里程计检查

​	2.配对一致性检查

​			参考文献【21】中提出,  Note that we use the simplified check (2) instead of the probabilistic check proposed in [21] since we do not have access to precise covariances for the odometry and loop closures.

​			使用领接矩阵来跟踪判断配对的回环检测是否一致？？？（什么意思）



系统向基站发送 位姿 和 点云图显示。

匹配中：两个匹配的模块或者帧构成一个里程估计，将这个里程估计传递给后端。





## discussion

LAMP在国防高级研究计划局的隧道电路中获得第二名

改进的点：

​	1.在参考文献中有更加先进的解决算法。

​		一。激光里程计 使用GICP算法会得到局部极小值，可以通过多模态融合信息来解决，或者使用其他优秀的显式的对感知退化模型。（这是一个很好的点）

​		二。GICP初始化还有改善的空间。提出了建议：基于特征的方法进行更加鲁棒的全局扫描匹配，而不是使用猜测的方法。 或者使用多模态进行回环检测。得到更加精确的协方差。

​		三。拒绝离群值。 可以开发一个更加鲁棒的全局解算器来提高slam的可靠性，并且针对特定问题进行细化的参数调整。

​		四。多机器人mapping系统。LAMP使用了集中式的系统。文献中有实现了完全分布式的系统，来解决通信瓶颈，减少通信带宽，提高实时性。

​	2.有基本的开放研究问题。











