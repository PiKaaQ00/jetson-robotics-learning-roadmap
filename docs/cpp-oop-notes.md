# C++ 面向对象基础复习笔记

这份笔记整理 C++ 面向对象编程中最核心的一组概念：`class`、`struct`、成员变量、成员函数、构造函数、析构函数、`this` 指针、封装、继承、多态、虚函数、纯虚函数和接口类。

这些知识在机器人软件中非常常见。例如电机控制器、相机驱动、传感器模块、路径规划器、任务调度器，通常都会用类来组织。

## 1. class 和 struct

`class` 和 `struct` 都可以用来定义自定义类型，都可以包含成员变量、成员函数、构造函数和析构函数。

主要区别：

```text
class 默认访问权限是 private
struct 默认访问权限是 public
```

示例：

```cpp
class Robot
{
    int battery; // 默认 private
};

struct Position
{
    int x;       // 默认 public
    int y;       // 默认 public
};
```

通常习惯：

```text
struct：主要用于简单数据组合，例如 Position、Velocity、SensorData。
class：主要用于有封装、有行为、有不变量的对象，例如 Robot、MotorController。
```

## 2. 成员变量

成员变量是属于对象的数据。

示例：

```cpp
class Robot
{
private:
    std::string name;
    int battery;
};
```

每个对象都有自己的一份成员变量：

```cpp
Robot r1;
Robot r2;
```

`r1` 和 `r2` 的 `name`、`battery` 是两份不同的数据。

## 3. 成员函数

成员函数是属于类的函数，用来操作对象的数据。

示例：

```cpp
class Robot
{
private:
    int battery;

public:
    int getBattery() const
    {
        return battery;
    }
};
```

调用：

```cpp
Robot robot;
robot.getBattery();
```

成员函数后面的 `const` 表示该函数不会修改当前对象：

```cpp
int getBattery() const;
```

## 4. 构造函数

构造函数用于创建对象时初始化对象。

特点：

```text
函数名和类名相同
没有返回类型
创建对象时自动调用
```

示例：

```cpp
class Robot
{
private:
    std::string name;
    int battery;

public:
    Robot(const std::string & robotName, int initialBattery)
        : name(robotName), battery(initialBattery)
    {
    }
};
```

这里：

```cpp
: name(robotName), battery(initialBattery)
```

叫成员初始化列表。它在构造函数体执行前初始化成员变量。

## 5. 析构函数

析构函数用于对象销毁时执行清理工作。

特点：

```text
函数名是 ~类名
没有返回类型
没有参数
对象销毁时自动调用
```

示例：

```cpp
class Robot
{
public:
    ~Robot()
    {
        std::cout << "Robot destroyed\n";
    }
};
```

如果类管理动态内存、文件、网络连接、硬件资源，析构函数通常负责释放资源。

在有继承和多态的类中，基类析构函数通常要写成 `virtual`：

```cpp
virtual ~Base() = default;
```

## 6. this 指针

`this` 是成员函数内部隐含存在的指针，指向当前对象。

示例：

```cpp
class Robot
{
private:
    int battery;

public:
    void setBattery(int battery)
    {
        this->battery = battery;
    }
};
```

这里：

```cpp
this->battery
```

表示成员变量。

右边的：

```cpp
battery
```

表示函数参数。

`this` 常用于区分同名参数和成员变量，也可用于返回当前对象：

```cpp
Robot & self()
{
    return *this;
}
```

## 7. 封装

封装是指把数据隐藏在类内部，只通过公开的成员函数访问和修改。

示例：

```cpp
class Robot
{
private:
    int battery;

public:
    void setBattery(int value)
    {
        if (value < 0)
            battery = 0;
        else if (value > 100)
            battery = 100;
        else
            battery = value;
    }

    int getBattery() const
    {
        return battery;
    }
};
```

好处：

```text
防止外部随意修改内部数据
保证对象状态合法
降低代码之间的耦合
方便以后修改内部实现
```

比如电量应该在 `0-100` 之间，就可以在 `setBattery()` 中统一限制。

## 8. 继承

继承表示一个类基于另一个类扩展。

示例：

```cpp
class Sensor
{
public:
    void start()
    {
        std::cout << "Sensor started\n";
    }
};

class Camera : public Sensor
{
public:
    void capture()
    {
        std::cout << "Capture image\n";
    }
};
```

使用：

```cpp
Camera cam;
cam.start();   // 来自 Sensor
cam.capture(); // Camera 自己的函数
```

`public` 继承通常表示 `is-a` 关系：

```text
Camera is a Sensor
相机是一种传感器
```

## 9. 多态

多态是指通过基类指针或引用调用虚函数时，实际执行派生类版本。

示例：

```cpp
Sensor * sensor = new Camera;
sensor->read();
```

如果 `read()` 是虚函数，并且 `Camera` 重写了它，那么调用的是：

```cpp
Camera::read()
```

而不是：

```cpp
Sensor::read()
```

多态的作用：

```text
用统一接口处理不同对象
降低模块之间的耦合
方便扩展新的派生类
```

在机器人项目中，可以用统一的 `Sensor *` 或 `Sensor &` 处理相机、激光雷达、IMU 等不同传感器。

## 10. 虚函数

虚函数用关键字 `virtual` 声明。

示例：

```cpp
class Sensor
{
public:
    virtual void read()
    {
        std::cout << "Read generic sensor\n";
    }
};
```

派生类重写：

```cpp
class Camera : public Sensor
{
public:
    void read() override
    {
        std::cout << "Read camera frame\n";
    }
};
```

`override` 不是必须的，但强烈建议使用。它可以让编译器检查你是否真的重写了基类虚函数。

例如，如果基类是：

```cpp
virtual void read() const;
```

派生类误写成：

```cpp
void read() override;
```

编译器会报错，因为少了 `const`。

## 11. 纯虚函数

纯虚函数是在虚函数后面写 `= 0`。

示例：

```cpp
class Sensor
{
public:
    virtual void read() = 0;
};
```

有纯虚函数的类叫抽象类，不能直接创建对象：

```cpp
// Sensor s; // 错误
```

派生类必须实现纯虚函数，否则派生类仍然是抽象类。

## 12. 接口类

接口类是一种主要由纯虚函数组成的抽象类，用于规定派生类必须提供哪些能力。

示例：

```cpp
class ISensor
{
public:
    virtual ~ISensor() = default;
    virtual void start() = 0;
    virtual void read() = 0;
    virtual void stop() = 0;
};
```

接口类的特点：

```text
通常没有普通成员变量
通常只有纯虚函数
通常有 virtual 析构函数
用于定义统一接口
```

实现接口：

```cpp
class Lidar : public ISensor
{
public:
    void start() override;
    void read() override;
    void stop() override;
};
```

这样调度器或机器人系统不需要知道具体是相机还是雷达，只需要面向 `ISensor` 编程。

## 13. 综合示例：机器人传感器系统

下面的示例把上面的知识点串起来。

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <vector>

struct Position
{
    double x;
    double y;

    void print() const
    {
        std::cout << "(" << x << ", " << y << ")\n";
    }
};

class ISensor
{
public:
    virtual ~ISensor() = default;
    virtual void start() = 0;
    virtual void read() = 0;
    virtual void stop() = 0;
    virtual std::string name() const = 0;
};

class SensorBase : public ISensor
{
private:
    std::string sensorName;
    bool running;

public:
    SensorBase(const std::string & name)
        : sensorName(name), running(false)
    {
        std::cout << "SensorBase constructor: " << sensorName << '\n';
    }

    virtual ~SensorBase()
    {
        std::cout << "SensorBase destructor: " << sensorName << '\n';
    }

    void start() override
    {
        running = true;
        std::cout << sensorName << " started\n";
    }

    void stop() override
    {
        running = false;
        std::cout << sensorName << " stopped\n";
    }

    std::string name() const override
    {
        return sensorName;
    }

protected:
    bool isRunning() const
    {
        return running;
    }
};

class CameraSensor : public SensorBase
{
private:
    int width;
    int height;

public:
    CameraSensor(const std::string & name, int width, int height)
        : SensorBase(name), width(width), height(height)
    {
        std::cout << "CameraSensor constructor\n";
    }

    ~CameraSensor() override
    {
        std::cout << "CameraSensor destructor\n";
    }

    void read() override
    {
        if (!isRunning())
        {
            std::cout << name() << " is not running\n";
            return;
        }

        std::cout << name() << " captured image "
                  << width << "x" << height << '\n';
    }
};

class LidarSensor : public SensorBase
{
private:
    int pointCount;

public:
    LidarSensor(const std::string & name, int pointCount)
        : SensorBase(name), pointCount(pointCount)
    {
        std::cout << "LidarSensor constructor\n";
    }

    ~LidarSensor() override
    {
        std::cout << "LidarSensor destructor\n";
    }

    void read() override
    {
        if (!isRunning())
        {
            std::cout << name() << " is not running\n";
            return;
        }

        std::cout << name() << " read "
                  << pointCount << " lidar points\n";
    }
};

class Robot
{
private:
    std::string robotName;
    int battery;
    Position position;
    std::vector<std::unique_ptr<ISensor>> sensors;

public:
    Robot(const std::string & name, int battery, Position start)
        : robotName(name), battery(battery), position(start)
    {
        std::cout << "Robot constructor: " << robotName << '\n';
    }

    ~Robot()
    {
        std::cout << "Robot destructor: " << robotName << '\n';
    }

    void setBattery(int battery)
    {
        if (battery < 0)
            this->battery = 0;
        else if (battery > 100)
            this->battery = 100;
        else
            this->battery = battery;
    }

    int getBattery() const
    {
        return battery;
    }

    void addSensor(std::unique_ptr<ISensor> sensor)
    {
        sensors.push_back(std::move(sensor));
    }

    void startAllSensors()
    {
        for (auto & sensor : sensors)
        {
            sensor->start();
        }
    }

    void readAllSensors()
    {
        for (auto & sensor : sensors)
        {
            sensor->read();
        }
    }

    void stopAllSensors()
    {
        for (auto & sensor : sensors)
        {
            sensor->stop();
        }
    }

    void printStatus() const
    {
        std::cout << "Robot: " << robotName << '\n';
        std::cout << "Battery: " << battery << "%\n";
        std::cout << "Position: ";
        position.print();
    }
};

int main()
{
    Position start{1.5, 2.0};

    Robot robot("JetsonBot", 85, start);
    robot.printStatus();

    robot.addSensor(std::make_unique<CameraSensor>("front_camera", 1280, 720));
    robot.addSensor(std::make_unique<LidarSensor>("top_lidar", 120000));

    robot.startAllSensors();
    robot.readAllSensors();
    robot.stopAllSensors();

    robot.setBattery(120);
    std::cout << "Battery after clamp: "
              << robot.getBattery() << "%\n";

    return 0;
}
```

## 14. 示例中的知识点对应

```text
class：Robot、SensorBase、CameraSensor、LidarSensor、ISensor
struct：Position
成员变量：robotName、battery、position、sensors、width、height、pointCount
成员函数：startAllSensors()、readAllSensors()、setBattery()、read()
构造函数：Robot(...)、SensorBase(...)、CameraSensor(...)
析构函数：~Robot()、~SensorBase()、~CameraSensor()
this 指针：this->battery = ...
封装：Robot 的 battery 是 private，只能通过 setBattery()/getBattery() 访问
继承：CameraSensor 和 LidarSensor 继承 SensorBase
多态：std::unique_ptr<ISensor> 指向不同派生类对象
虚函数：read()、start()、stop()、name()
纯虚函数：ISensor 中的 virtual void read() = 0
接口类：ISensor
```

## 15. 复习口诀

```text
class 管行为，struct 管简单数据。
成员变量保存状态，成员函数操作状态。
构造函数负责初始化，析构函数负责清理。
this 指向当前对象。
封装隐藏细节，只暴露必要接口。
继承表达 is-a 关系。
虚函数支持运行时多态。
纯虚函数让类变成抽象类。
接口类规定能力，不关心实现。
```

