# auto (C++)

微软官网 https://learn.microsoft.com/zh-cn/cpp/cpp/auto-cpp?view=msvc-170


```cpp
#include <iostream>

using namespace std;

int main( )
{
    int count = 10;
    int& countRef = count;
    auto myAuto = countRef;

    countRef = 11;
    cout << count << " ";

    myAuto = 12;
    cout << count << endl;
}
```
auto不能代表一个实际的类型声明,只是一个类型声明在编写代码阶段的"占位符",
在使用auto变量时必须立刻初始化,让编译器立刻推断出他的实际类型,并在编译阶段将auto占位符替换为真正的类型
```cpp
#include <iostream>
#include <utility>

// 接受左值引用的函数
void process(int& x) {
  std::cout << "process(int&): " << x << std::endl;
}

// 接受右值引用的函数
void process(int&& x) {
  std::cout << "process(int&&): " << x << std::endl;
}

// 接受任意类型的参数
template <typename T>
void forward(T&& x) {
  process(std::forward<T>(x));  // 转发参数到 process 函数
}

int main() {
  int n = 42;
  forward(n);   // 接受左值
  forward(100); // 接受右值
  return 0;
}

```

在上面的代码中，`forward`函数使用了`auto&&`来实现类型转发，将参数`x`转发给了`process`函数。由于`auto&&`的引用折叠特性，当`forward`函数接受一个左值时，`auto&&`推导出的类型是左值引用，这样转发的参数就是左值引用；而当`forward`函数接受一个右值时，`auto&&`推导出的类型是右值引用，这样转发的参数就是右值引用。

在上面的例子中，当`forward`函数接受一个左值时，`process(int&)`会被调用；而当`forward`函数接受一个右值时，`process(int&&)`会被调用。这就是引用折叠规则的应用