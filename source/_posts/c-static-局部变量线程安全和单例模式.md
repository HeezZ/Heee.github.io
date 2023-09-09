---
title: c++ static 局部变量线程安全和单例模式
date: 2023-07-26 23:10:08
tags:
---

#### effective c++ 中的描述
p32: 任何一种non-const static 对象，不论他是local 还是 non-local，在多线程环境下“等待某事发生”都会有麻烦

推测：此书中认为局部变量的static 是非线程安全的

#### 实验

编写以下代码：
```c++
#include <iostream>
#include <thread>

class Singleton
{
public:
static Singleton& Instance()
{
    static Singleton singleton;
    std::cout << &singleton << std::endl;   // std::cout 非线程安全，仅用于测试，忽略
    return singleton;
}

private:

Singleton() 
{
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
};

};


int main()
{
    std::thread th1(Singleton::Instance);
    std::thread th2(Singleton::Instance);
    std::thread th3(Singleton::Instance);
    std::thread th4(Singleton::Instance);

    th1.join();
    th2.join();
    th3.join();
    th4.join();
}
```

得到以下输出：
```shell
0x104304000
0x104304000
0x104304000
0x104304000
```

与书中描述不符

#### 结论

C++11 保证静态局部变量的初始化过程是线程安全的。

参考：
https://www.zhihu.com/question/267013757
https://stackoverflow.com/questions/34457432/c11-singleton-static-variable-is-thread-safe-why
https://www.cnblogs.com/william-cheung/p/4831085.html

