# Jetson 机器人开发学习路线

本路线面向 Nvidia Jetson 平台上的机器人感知、控制、部署和 Demo 展示。

## 岗位能力目标

需要逐步具备：

- Jetson 平台环境配置能力
- 嵌入式 Linux 开发能力
- ROS1 / ROS2 机器人开发能力
- 传感器与执行器适配能力
- 深度学习模型部署能力
- TensorRT 推理加速能力
- 强化学习和仿真项目复现能力
- 技术文档和 Demo 输出能力

## 第一部分：Jetson 基础

需要学习：

- Jetson Orin / Xavier / Nano 基础
- JetPack 安装
- Ubuntu 系统配置
- CUDA
- cuDNN
- TensorRT
- jtop
- tegrastats
- Python 环境
- ROS2 环境

目标：

- 能刷机和配置 Jetson
- 能安装基础开发环境
- 能查看 CPU、GPU、内存和功耗
- 能运行基础 CUDA / TensorRT 示例

## 第二部分：嵌入式 Linux

需要学习：

- Linux 文件系统
- 用户和权限
- systemd 服务
- udev 规则
- 环境变量
- 进程管理
- 网络配置
- SSH
- 串口工具
- 日志查看

目标：

- 能通过 SSH 管理 Jetson
- 能配置设备权限
- 能让程序开机自启动
- 能排查设备连接问题

## 第三部分：传感器接入

常见传感器：

- USB 摄像头
- CSI 摄像头
- RealSense 深度相机
- 激光雷达
- IMU
- 编码器
- GPS

需要学习：

- V4L2
- OpenCV 读取相机
- ROS2 camera 驱动
- LiDAR ROS 驱动
- IMU 数据格式
- rosbag 数据录制
- TF 坐标变换

目标：

- 能让传感器在 Jetson 上正常工作
- 能把传感器数据发布到 ROS2
- 能用 RViz 查看数据
- 能录制和回放数据

## 第四部分：执行器和通信

常见执行器：

- 电机
- 舵机
- 移动底盘
- 机械臂
- 四足机器人关节

需要学习：

- 串口通信
- CAN 通信
- I2C / SPI 基础
- Socket 通信
- PWM
- GPIO
- 控制指令协议
- 超时和重连机制

目标：

- 能发送控制指令
- 能读取执行器状态
- 能处理通信异常
- 能封装基础驱动节点

## 第五部分：ROS2 系统开发

需要学习：

- rclcpp
- rclpy
- Topic
- Service
- Action
- Launch
- Parameter
- QoS
- TF2
- URDF / Xacro
- RViz
- Nav2
- MoveIt

目标：

- 能搭建机器人 ROS2 系统
- 能把传感器、控制器和算法串起来
- 能调试导航、感知和控制模块
- 能写部署文档

## 第六部分：模型部署与加速

需要学习：

- PyTorch
- OpenCV
- ONNX
- TensorRT
- FP16 / INT8
- batch size
- latency
- throughput
- CUDA 基础

目标：

- 能把 PyTorch 模型导出 ONNX
- 能构建 TensorRT engine
- 能在 Jetson 上进行实时推理
- 能比较优化前后的性能

推荐 Demo：

- Jetson + YOLO 目标检测
- Jetson + 深度估计
- Jetson + 图像分割
- Jetson + ROS2 视觉节点

## 第七部分：机器人算法

需要学习：

- 目标检测
- 图像分割
- 深度估计
- SLAM
- 定位导航
- 传感器融合
- 路径规划
- PID
- MPC 基础

目标：

- 能理解常见机器人算法模块
- 能复现开源项目
- 能把算法集成到 ROS2
- 能在 Jetson 上跑 Demo

## 第八部分：强化学习和仿真

需要学习：

- 强化学习基础
- PPO
- rsl_rl
- rs_rl
- Isaac Gym
- Isaac Sim
- MuJoCo
- Genesis
- 模仿学习
- Real2Sim
- Sim2Real

目标：

- 能复现仿真训练项目
- 能修改奖励函数
- 能调整机器人环境参数
- 能导出训练结果
- 能理解从仿真到真实部署的基本流程

## 推荐作品集

最终建议完成：

```text
1. Jetson + ROS2 + YOLO / TensorRT 视觉感知 Demo
2. Jetson + ROS2 + LiDAR + Nav2 自主导航 Demo
3. Isaac Gym / MuJoCo + rsl_rl 强化学习训练 Demo
4. 多传感器融合机器人 Demo
5. 完整 GitHub README + 部署文档 + Demo 视频
```

