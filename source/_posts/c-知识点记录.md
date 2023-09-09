---
title: c++知识点记录
date: 2023-02-28 00:22:18
tags: c++
---

- shared_ptr 析构时引用计数先减一，后释放内存
``` c++
#include <memory>
#include <iostream>
#include <unistd.h>
#include <thread>

class Foo
{
public:
    Foo() = default;
    ~Foo()
    {
        sleep(5);
    }
private:
    int a = 0;
};

int main()
{
    std::shared_ptr<Foo> f(std::make_shared<Foo>());
    std::weak_ptr<Foo> w(f);
    
    auto release = [](std::shared_ptr<Foo> f) {
        sleep(1);
        f = nullptr;
        std::cout << "f is released" << std::endl;
    };
    
    std::thread t(release, f);
    t.detach();

    f = nullptr;
    std::cout << "f is set to nullptr" << std::endl;
    if (w.expired()) {
        std::cout << "w is expired" << std::endl;
    } else {
        std::cout << "w is not expired yet" << std::endl;
    }

    sleep(2);
    if (w.expired()) {
        std::cout << "w is expired" << std::endl;
    }

    sleep(10);
    
}

```
输出
```
f is set to nullptr
w is not expired yet
w is expired
f is released
```
上面的程序通过weak_ptr 来判断引用计数先减一还是先释放内存，若引用计数为0，expired 返回 true.