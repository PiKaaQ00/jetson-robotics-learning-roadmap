# C++ 左值、右值与移动语义复习笔记

这份笔记用于复习 C++11 中和性能、资源管理密切相关的一组概念：左值、右值、右值引用、移动构造函数、移动赋值运算符以及 `std::move()`。

## 1. 左值和右值

简单理解：

```text
左值：有身份、有名字、通常能取地址，表达式结束后还继续存在。
右值：临时值、通常没有名字，表达式结束后就会销毁。
```

示例：

```cpp
#include <iostream>
#include <string>

std::string makeName()
{
    return std::string("robot");
}

int main()
{
    int x = 10;

    x = 20;           // x 是左值
    int * p = &x;     // x 能取地址

    // &10;           // 错误：10 是右值，不能取地址
    // &(x + 1);      // 错误：x + 1 是临时结果，是右值

    std::string a = "sensor";  // a 是左值
    std::string b = makeName(); // makeName() 的返回值是右值

    std::cout << *p << '\n';
    std::cout << a << '\n';
    std::cout << b << '\n';
}
```

常见判断：

```cpp
int x = 1;

x;          // 左值
10;         // 右值
x + 1;      // 右值
std::string("imu"); // 右值
```

注意：这是面向学习的简化判断。现代 C++ 还有更细的值类别，如 glvalue、prvalue、xvalue。

## 2. 左值引用和右值引用

左值引用：

```cpp
int x = 10;
int & r = x;
r = 20;
```

`r` 是 `x` 的别名。

右值引用：

```cpp
int && rr = 10;
```

`rr` 可以绑定到右值。右值引用主要用于移动语义。

对于类：

```cpp
class Buffer
{
public:
    Buffer(const Buffer & other); // 复制构造：接收左值
    Buffer(Buffer && other);      // 移动构造：接收右值
};
```

调用规则：

```cpp
Buffer a;
Buffer b(a);             // a 是左值，调用复制构造
Buffer c(Buffer{});      // Buffer{} 是右值，调用移动构造，或被编译器直接优化
```

## 3. 为什么需要移动语义

如果一个类管理堆内存，复制对象通常要进行深拷贝：

```text
重新申请内存
复制所有数据
两个对象各自拥有一份资源
```

但如果源对象是临时对象，马上就要销毁，深拷贝很浪费。

移动语义的想法是：

```text
不复制大块数据；
直接接管临时对象内部的资源；
把临时对象置为空状态。
```

类似把文件从一个目录移动到另一个目录，通常只是修改记录，不是真的复制全部文件内容。

## 4. 一个完整示例：深拷贝和移动

```cpp
#include <cstring>
#include <iostream>
#include <utility>

class Buffer
{
private:
    char * data;
    std::size_t size;

public:
    Buffer(const char * text = "")
        : size(std::strlen(text))
    {
        data = new char[size + 1];
        std::strcpy(data, text);
        std::cout << "constructor: " << data << '\n';
    }

    ~Buffer()
    {
        std::cout << "destructor: " << (data ? data : "null") << '\n';
        delete[] data;
    }

    Buffer(const Buffer & other)
        : size(other.size)
    {
        data = new char[size + 1];
        std::strcpy(data, other.data);
        std::cout << "copy constructor: " << data << '\n';
    }

    Buffer(Buffer && other) noexcept
        : data(other.data), size(other.size)
    {
        other.data = nullptr;
        other.size = 0;
        std::cout << "move constructor\n";
    }

    Buffer & operator=(const Buffer & other)
    {
        std::cout << "copy assignment\n";

        if (this == &other)
            return *this;

        char * newData = new char[other.size + 1];
        std::strcpy(newData, other.data);

        delete[] data;
        data = newData;
        size = other.size;

        return *this;
    }

    Buffer & operator=(Buffer && other) noexcept
    {
        std::cout << "move assignment\n";

        if (this == &other)
            return *this;

        delete[] data;

        data = other.data;
        size = other.size;

        other.data = nullptr;
        other.size = 0;

        return *this;
    }

    const char * c_str() const
    {
        return data ? data : "";
    }
};

Buffer makeBuffer()
{
    Buffer temp("temporary robot data");
    return temp;
}

int main()
{
    Buffer one("lidar data");

    Buffer two = one;          // one 是左值，调用复制构造
    Buffer three = makeBuffer(); // 返回值是右值，可能移动，也可能被编译器直接消除复制

    Buffer four("old camera data");
    four = one;                // one 是左值，调用复制赋值

    Buffer five("old imu data");
    five = std::move(one);     // 强制把 one 当右值，调用移动赋值

    std::cout << "two: " << two.c_str() << '\n';
    std::cout << "three: " << three.c_str() << '\n';
    std::cout << "four: " << four.c_str() << '\n';
    std::cout << "five: " << five.c_str() << '\n';
    std::cout << "one after move: " << one.c_str() << '\n';
}
```

## 5. 复制构造和移动构造

复制构造：

```cpp
Buffer(const Buffer & other)
```

用途：

```cpp
Buffer one("camera");
Buffer two = one;
```

`one` 是左值，复制后 `one` 和 `two` 都要继续有效，所以必须深拷贝。

移动构造：

```cpp
Buffer(Buffer && other) noexcept
```

用途：

```cpp
Buffer three = makeBuffer();
```

如果返回值没有被编译器直接优化掉，就可以调用移动构造，把临时对象的资源转给 `three`。

移动构造一般要做三件事：

```cpp
data = other.data;
size = other.size;
other.data = nullptr;
other.size = 0;
```

不能让两个对象同时拥有同一块堆内存，否则析构时会重复 `delete[]`。

## 6. 复制赋值和移动赋值

复制赋值用于已有对象接收左值：

```cpp
Buffer a("a");
Buffer b("b");

b = a; // 复制赋值
```

移动赋值用于已有对象接收右值：

```cpp
Buffer a("a");
Buffer b("b");

b = std::move(a); // 移动赋值
```

移动赋值常见步骤：

```text
1. 处理自我赋值
2. 释放目标对象原来的资源
3. 接管源对象的资源
4. 将源对象置为空状态
5. 返回 *this
```

## 7. std::move()

`std::move()` 在头文件 `<utility>` 中。

它的作用不是移动数据，而是做类型转换：

```text
把左值转换成右值引用，让移动构造或移动赋值有机会被调用。
```

示例：

```cpp
Buffer a("robot state");
Buffer b(std::move(a));
```

`a` 本来是左值。使用 `std::move(a)` 后，它可以匹配：

```cpp
Buffer(Buffer && other)
```

注意：

```text
std::move(a) 之后，a 仍然是有效对象；
但 a 的内容处于有效但未指定状态；
可以销毁 a，也可以重新赋值给 a；
不要继续依赖 a 原来的内容。
```

## 8. std::move() 不保证一定移动

如果类没有移动构造或移动赋值，`std::move()` 也可能退回到复制。

```cpp
class Data
{
public:
    Data(const Data & other);
    // 没有 Data(Data &&)
};

Data a;
Data b(std::move(a)); // 可能调用复制构造
```

原因是右值可以绑定到：

```cpp
const Data &
```

所以 `std::move()` 的准确含义是：

```text
允许移动，而不是保证移动。
```

## 9. 函数模板中的 T&&：转发引用

普通函数中的右值引用参数只能绑定右值：

```cpp
void doStuff(Buffer && buffer);

Buffer a("camera");

doStuff(Buffer("lidar")); // 可以，临时对象是右值
// doStuff(a);            // 错误，a 是左值
```

但函数模板中的 `T&&` 比较特殊：

```cpp
template <typename T>
void foo(T && value)
{
}
```

当 `T` 需要由实参推导出来，并且参数形式正好是 `T&&` 时，它不是普通右值引用，而是**转发引用**，也常被称为**万能引用**。

它的规则是：

```text
传入左值：T 被推导为左值引用。
传入右值：T 被推导为普通类型。
```

示例：

```cpp
#include <string>

template <typename T>
void foo(T && value)
{
}

int main()
{
    int i = 42;

    foo(i);             // i 是左值，T 推导为 int&
    foo(42);            // 42 是右值，T 推导为 int
    foo(std::string{}); // 临时 string 是右值，T 推导为 std::string
}
```

当传入左值时：

```cpp
foo(i);
```

模板推导结果是：

```cpp
T = int&
```

参数类型：

```cpp
T&&
```

会变成：

```cpp
int& &&
```

这时会发生引用折叠。

引用折叠规则：

```text
&  + &  -> &
&  + && -> &
&& + &  -> &
&& + && -> &&
```

只要出现一个左值引用 `&`，结果就是左值引用。

所以：

```cpp
int& &&
```

最终折叠为：

```cpp
int&
```

因此 `foo(i)` 最终相当于：

```cpp
void foo(int & value);
```

这就是为什么模板中的 `T&&` 可以接收左值。

## 10. std::forward：保留原来的左值/右值属性

即使参数类型是右值引用，只要参数有名字，在函数内部它就是左值表达式。

```cpp
template <typename T>
void foo(T && value)
{
    // value 有名字，所以 value 在这里是左值
}
```

如果要把参数继续传给别的函数，并保留它原来的左值或右值属性，应使用：

```cpp
std::forward<T>(value)
```

完整示例：

```cpp
#include <iostream>
#include <utility>

void target(int &)
{
    std::cout << "target(int&): left value\n";
}

void target(int &&)
{
    std::cout << "target(int&&): right value\n";
}

template <typename T>
void wrapper(T && value)
{
    target(std::forward<T>(value));
}

int main()
{
    int x = 10;

    wrapper(x);  // x 是左值，调用 target(int&)
    wrapper(20); // 20 是右值，调用 target(int&&)
}
```

如果在 `wrapper()` 中直接写：

```cpp
target(value);
```

那么两次都会调用：

```cpp
target(int&)
```

因为 `value` 有名字，所以在函数体内部是左值。

`std::move()` 和 `std::forward()` 的区别：

```text
std::move(x)：无条件把 x 转成右值。
std::forward<T>(x)：根据 T 的推导结果决定保留左值还是右值。
```

所以：

```text
移动语义中，明确要转移资源时用 std::move。
模板转发参数时，想保留原始值类别时用 std::forward。
```

## 11. noexcept 的意义

移动构造和移动赋值通常建议加 `noexcept`：

```cpp
Buffer(Buffer && other) noexcept;
Buffer & operator=(Buffer && other) noexcept;
```

原因是标准库容器在扩容时更愿意使用不会抛异常的移动操作。

例如 `std::vector<Buffer>` 扩容时，如果移动构造不是 `noexcept`，为了异常安全，它可能选择复制而不是移动。

## 12. 和机器人项目的关系

机器人 C++ 项目中，经常会有大对象：

```text
图像帧
点云数据
传感器缓存
路径点数组
地图数据
模型推理结果
```

这些对象如果频繁深拷贝，会影响实时性。移动语义可以减少不必要的数据复制。

示例：

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <vector>

struct SensorFrame
{
    std::vector<float> data;
    std::string frameId;
};

SensorFrame readFrame()
{
    SensorFrame frame;
    frame.frameId = "lidar";
    frame.data.resize(1000000, 1.0f);
    return frame;
}

int main()
{
    SensorFrame frame = readFrame(); // 返回值可移动或被直接构造

    std::vector<SensorFrame> buffer;
    buffer.push_back(std::move(frame)); // 避免复制大 vector

    std::cout << buffer[0].frameId << '\n';
}
```

## 13. 记忆总结

```text
左值：有名字，能继续使用。
右值：临时对象，用完就销毁。
T&：左值引用。
T&&：右值引用。
模板参数 T&&：在类型推导场景下是转发引用。
复制构造：用左值创建新对象，深拷贝。
移动构造：用右值创建新对象，接管资源。
复制赋值：已有对象接收左值，深拷贝。
移动赋值：已有对象接收右值，释放旧资源并接管新资源。
std::move：把左值转换成右值引用，允许移动。
std::forward<T>：保留模板参数原来的左值/右值属性。
移动后对象：仍然有效，但不要依赖原来的内容。
```
