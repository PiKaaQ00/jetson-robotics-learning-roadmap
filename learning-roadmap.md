# 总体学习路线

本路线以适配 Nvidia Jetson 机器人开发岗位为目标，重点覆盖编程、Linux、ROS2、Jetson、深度学习、强化学习、仿真平台和开源项目转化。

## 第一阶段：基础能力

周期建议：第 1 个月

需要掌握：

- C++ 基础语法
- Python 基础语法
- Linux 命令行
- Git / GitHub
- Markdown 文档编写

阶段目标：

- 能写基础 C++ 小程序
- 能写 Python 脚本
- 能使用 Linux 命令行管理文件、进程和环境
- 能使用 Git 提交代码
- 能写清晰的 README 文档

阶段产出：

- C++ 练习代码
- Python 数据处理脚本
- 一份个人学习路线 README
- 每周学习总结

## 第二阶段：C++ 工程能力

周期建议：第 1-2 个月

需要掌握：

- 面向对象编程
- 指针、引用、const
- STL 标准库
- 智能指针
- 现代 C++11/14/17
- CMake
- 多文件工程组织
- gdb 调试

阶段目标：

- 能写 class
- 能使用 vector、map、string 等常用容器
- 能理解 shared_ptr、unique_ptr
- 能写 CMakeLists.txt
- 能排查基础编译和运行错误

阶段产出：

- 电机控制器类 MotorController
- 传感器数据缓存类 SensorBuffer
- 多文件 C++ 小工程
- CMake 编译示例项目

## 第三阶段：ROS2 机器人开发

周期建议：第 2-3 个月

需要掌握：

- ROS2 工作空间
- package 创建
- Topic / Service / Action
- Publisher / Subscriber
- Launch
- Parameter
- TF / TF2
- RViz
- rosbag
- QoS
- 自定义 msg / srv / action

阶段目标：

- 能写 ROS2 C++ 节点
- 能写 ROS2 Python 节点
- 能接入模拟传感器数据
- 能发布机器人控制指令
- 能用 RViz 调试系统状态

阶段产出：

- ROS2 传感器发布节点
- ROS2 电机控制订阅节点
- 自定义消息类型
- 一个简单机器人数据流 Demo

## 第四阶段：Jetson 平台部署

周期建议：第 3-4 个月

需要掌握：

- JetPack 安装与配置
- CUDA / cuDNN / TensorRT 基础
- 摄像头接入
- 深度相机接入
- LiDAR 接入
- IMU 接入
- 串口 / CAN 通信
- 性能监控与调优

阶段目标：

- 能配置 Jetson 开发环境
- 能在 Jetson 上运行 ROS2
- 能接入至少一种传感器
- 能部署一个视觉模型
- 能分析 CPU、GPU、内存和延迟问题

阶段产出：

- Jetson + ROS2 + 摄像头 Demo
- Jetson + YOLO 推理 Demo
- Jetson 性能测试记录
- 部署文档

## 第五阶段：深度学习部署

周期建议：第 4-5 个月

需要掌握：

- PyTorch
- OpenCV
- ONNX
- TensorRT
- 模型量化
- 推理加速
- 目标检测
- 图像分割
- 深度估计

阶段目标：

- 能训练或复现基础视觉模型
- 能导出 ONNX
- 能使用 TensorRT 加速推理
- 能在 Jetson 上跑实时感知 Demo

阶段产出：

- YOLO / 分割模型部署 Demo
- PyTorch 到 ONNX 转换记录
- TensorRT 推理脚本
- 推理性能对比文档

## 第六阶段：仿真与强化学习

周期建议：第 5-6 个月

需要掌握：

- 强化学习基础
- PPO
- 模仿学习基础
- Isaac Gym
- MuJoCo
- Genesis
- rsl_rl / rs_rl
- Sim2Real / Real2Sim 基础

阶段目标：

- 能复现一个强化学习训练项目
- 能修改奖励函数
- 能调整环境参数
- 能记录训练曲线和实验结果

阶段产出：

- Isaac Gym 或 MuJoCo 复现项目
- rsl_rl 训练记录
- 奖励函数修改说明
- 仿真 Demo 视频或截图

## 第七阶段：作品集与开源转化

周期建议：第 7-12 个月

重点目标：

- 做完整项目
- 写完整文档
- 准备 GitHub 作品集
- 参与开源社区
- 形成可展示 Demo

推荐作品：

- Jetson + ROS2 + YOLO / TensorRT 视觉感知 Demo
- Jetson + ROS2 + LiDAR + Nav2 自主导航 Demo
- Isaac Gym / MuJoCo + rsl_rl 强化学习训练 Demo
- 多传感器融合机器人 Demo

