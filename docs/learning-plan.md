# 每日与阶段学习计划

本计划按每天 10 小时学习强度设计，目标是在 4-6 个月内基本适配 Jetson 机器人开发岗位。

## 每日时间分配

```text
08:00-10:00  理论学习
10:00-13:00  代码练习
14:00-17:00  项目实战
17:00-18:00  英文文档 / 开源项目阅读
20:00-21:00  学习总结 / GitHub 提交 / README 编写
```

可以根据个人作息调整，但每天必须保留：

- 代码练习
- 项目实战
- 文档总结
- GitHub 提交

## 每周固定产出

每周至少完成：

- 一个可运行小 Demo
- 一篇学习笔记
- 一次 GitHub 提交
- 一个问题排查记录
- 一个 README 或技术文档更新

## 第 1 个月：C++、Python、Linux、Git

学习重点：

- C++ 基础语法
- Python 基础语法
- Linux 常用命令
- Git / GitHub
- Markdown

每周安排：

```text
第1周：C++基础语法 + Linux基础
第2周：C++函数、数组、字符串 + Git
第3周：C++面向对象 + Python基础
第4周：C++指针、引用、const + Markdown文档
```

月度产出：

- C++ 小项目 2 个
- Python 脚本 2 个
- GitHub 学习仓库
- 学习路线 README

## 第 2 个月：C++ 工程能力

学习重点：

- 面向对象
- 智能指针
- STL
- 现代 C++
- CMake
- 多文件工程
- gdb 调试

每周安排：

```text
第5周：类、继承、多态
第6周：智能指针、RAII、内存管理
第7周：STL、lambda、algorithm
第8周：CMake、多文件工程、gdb
```

月度产出：

- MotorController 类
- SensorBuffer 类
- CMake 示例工程
- 多文件机器人模块 Demo

## 第 3 个月：ROS2 入门与机器人基础

学习重点：

- ROS2 工作空间
- Topic
- Service
- Action
- Launch
- Parameter
- TF
- RViz
- rosbag

每周安排：

```text
第9周：ROS2环境和Topic
第10周：Service、Action、Parameter
第11周：TF、URDF、RViz
第12周：rosbag、自定义消息、综合Demo
```

月度产出：

- ROS2 C++ 发布订阅节点
- ROS2 Python 节点
- 自定义 msg / srv
- 简单机器人数据流 Demo

## 第 4 个月：Jetson 与传感器

学习重点：

- JetPack
- CUDA / TensorRT 基础
- 摄像头接入
- 深度相机接入
- LiDAR 接入
- IMU 接入
- 串口 / CAN 基础

每周安排：

```text
第13周：Jetson系统配置和性能监控
第14周：摄像头和OpenCV
第15周：LiDAR / IMU / rosbag
第16周：串口 / CAN 通信和传感器综合Demo
```

月度产出：

- Jetson + ROS2 + 摄像头 Demo
- Jetson + LiDAR 数据显示 Demo
- IMU 数据发布节点
- 传感器接入文档

## 第 5 个月：深度学习部署

学习重点：

- PyTorch
- OpenCV
- ONNX
- TensorRT
- YOLO
- 模型量化
- 推理性能分析

每周安排：

```text
第17周：PyTorch和OpenCV基础
第18周：YOLO模型推理
第19周：ONNX导出和验证
第20周：TensorRT加速和性能对比
```

月度产出：

- YOLO 推理 Demo
- ONNX 转换脚本
- TensorRT engine 构建记录
- 推理性能对比文档

## 第 6 个月：强化学习与仿真

学习重点：

- 强化学习基础
- PPO
- Isaac Gym
- MuJoCo
- Genesis
- rsl_rl
- rs_rl
- 模仿学习基础

每周安排：

```text
第21周：强化学习基础和PPO
第22周：Isaac Gym环境复现
第23周：MuJoCo或Genesis复现
第24周：rsl_rl训练和实验记录
```

月度产出：

- 强化学习训练 Demo
- 奖励函数修改记录
- 训练曲线截图
- 仿真项目 README

## 第 7-12 个月：作品集阶段

重点不是继续堆知识点，而是做完整项目。

推荐项目：

- Jetson + ROS2 + YOLO / TensorRT 视觉感知 Demo
- Jetson + ROS2 + LiDAR + Nav2 自主导航 Demo
- Isaac Gym / MuJoCo + rsl_rl 强化学习训练 Demo
- 多传感器融合机器人 Demo

每个项目都需要包含：

- 可运行代码
- README
- 环境安装说明
- 运行命令
- 常见问题
- Demo 视频或截图
- 性能测试结果

## 每天结束前检查

每天结束前回答以下问题：

```text
1. 今天学了什么？
2. 今天写了什么代码？
3. 今天遇到什么问题？
4. 问题是怎么解决的？
5. 今天有没有提交 GitHub？
6. 明天继续做什么？
```

## 判断是否进步的标准

不要只看学习时长，要看产出。

有效学习应该产生：

- 能运行的代码
- 可复现的 Demo
- 清晰的错误记录
- 技术文档
- GitHub 提交历史
- 阶段性作品集

