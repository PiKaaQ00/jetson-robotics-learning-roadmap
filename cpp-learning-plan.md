# C++ 学习计划

本计划面向机器人、ROS2、Jetson 和实时控制方向。学习重点不是算法竞赛，而是工程开发能力。

## 学习目标

最终需要达到：

- 能看懂机器人开源项目中的 C++ 代码
- 能写 ROS2 C++ 节点
- 能封装传感器驱动类
- 能处理电机、底盘、机械臂等控制逻辑
- 能使用 CMake 管理项目
- 能排查崩溃、卡顿、内存问题

## 第一阶段：C++ 基础语法

需要学习：

- 变量、常量
- 数据类型
- 运算符
- 条件判断
- 循环
- 函数
- 数组
- 字符串
- 作用域
- 头文件和源文件
- 输入输出

练习项目：

- 机器人电池电量计算器
- 传感器数据平均值计算
- 简单成绩管理系统

阶段标准：

- 能独立写小程序
- 能使用 g++ 编译运行
- 能把代码拆分成 `.hpp` 和 `.cpp`

## 第二阶段：面向对象编程

需要学习：

- class / struct
- 成员变量
- 成员函数
- 构造函数
- 析构函数
- this 指针
- 封装
- 继承
- 多态
- 虚函数
- 纯虚函数
- 接口类

练习项目：

- MotorController 电机控制器类
- CameraDriver 相机驱动类
- RobotState 机器人状态类
- Planner 路径规划器接口类

阶段标准：

- 能用类描述机器人模块
- 能理解接口和继承
- 能看懂常见机器人 C++ 类结构

## 第三阶段：指针、引用和内存管理

需要学习：

- 指针
- 引用
- const
- 指针和数组
- 动态内存 new / delete
- 栈内存和堆内存
- 浅拷贝和深拷贝
- 拷贝构造函数
- 赋值运算符
- RAII
- 智能指针

重点掌握：

- std::shared_ptr
- std::unique_ptr
- std::weak_ptr

练习项目：

- 传感器对象生命周期管理
- 机器人模块共享状态对象
- 使用 unique_ptr 管理驱动实例

阶段标准：

- 理解内存生命周期
- 能避免常见野指针问题
- 能读懂 ROS2 中的 SharedPtr 写法

## 第四阶段：STL 标准库

需要学习：

- std::vector
- std::array
- std::string
- std::map
- std::unordered_map
- std::queue
- std::deque
- std::set
- std::pair
- std::tuple
- 迭代器
- 范围 for
- lambda 表达式
- std::function
- std::bind
- algorithm 常用函数

练习项目：

- 传感器数据缓存队列
- 多机器人状态表
- 控制命令队列
- 路径点 vector 管理

阶段标准：

- 能熟练使用 vector、map、queue、string
- 能用 STL 管理机器人数据
- 能理解回调函数和 lambda

## 第五阶段：现代 C++

需要学习 C++11/14/17：

- auto
- nullptr
- enum class
- constexpr
- lambda
- 右值引用
- move 语义
- std::move
- std::chrono
- std::optional
- std::variant

练习项目：

- 使用 chrono 实现控制周期计时器
- 使用 optional 表示可能无效的传感器数据
- 使用 move 优化大数组数据传递

阶段标准：

- 能读懂现代 C++ 项目代码
- 能写出简洁、安全的工程代码

## 第六阶段：CMake 工程构建

需要学习：

- g++
- CMakeLists.txt
- add_executable
- add_library
- target_include_directories
- target_link_libraries
- find_package
- Debug / Release
- 静态库
- 动态库

后续 ROS2 相关：

- ament_cmake
- colcon build

练习项目：

- 多文件 C++ 工程
- sensor_driver 静态库
- robot_demo 可执行文件

阶段标准：

- 能独立写 CMakeLists.txt
- 能解决头文件路径和链接错误
- 能组织小型 C++ 工程

## 第七阶段：多线程与并发

需要学习：

- std::thread
- std::mutex
- std::lock_guard
- std::unique_lock
- std::atomic
- std::condition_variable
- 线程安全队列
- 定时任务

练习项目：

- 一个线程读取传感器数据
- 一个线程处理数据
- 一个线程发送控制命令
- 一个线程记录日志

阶段标准：

- 能理解线程安全
- 能避免数据竞争
- 能写基础多线程机器人程序

## 第八阶段：通信和调试

需要学习：

- 文件读写
- JSON / YAML 配置
- 日志系统
- 异常处理
- 串口通信
- Socket 通信
- CAN 通信基础
- 二进制数据解析
- CRC 校验
- gdb
- valgrind
- perf

练习项目：

- 串口数据帧解析器
- 机器人状态日志系统
- 简单 TCP 控制服务
- 模拟电机通信协议

阶段标准：

- 能定位 Segmentation fault
- 能排查程序卡住和死锁
- 能解析简单二进制协议
- 能写基础驱动通信模块

## C++ 学习顺序

```text
1. 基础语法
2. 函数、数组、字符串
3. class 和面向对象
4. 指针、引用、const
5. 内存管理和智能指针
6. STL
7. lambda 和现代 C++
8. CMake
9. 多线程
10. 通信和调试
```

## 进入 ROS2 C++ 的标准

达到以下水平后，可以开始学习 ROS2 C++：

- 能写 class
- 能用 vector / map / string
- 理解指针和引用
- 会用 shared_ptr / unique_ptr
- 会写 CMakeLists.txt
- 知道怎么调试段错误
- 能写一个多文件 C++ 项目

