# 快速开始
## 下载源代码
下载地址：https://github.com/muflihun/easyloggingpp
下载完并解压
## 配置
Easylogging是高效的单头文件日志库，并且不需要lib或dll。
只需要在项目中包含头文件`easylogging++.h`和实现`easylogging++.cc`，即完成所有配置了。
## 最简例子
```c++
#include "easylogging++.h"

INITIALIZE_EASYLOGGINGPP    // 初始化宏，有且只能使用一次

int main(int argc, char* argv[]) {
   LOG(INFO) << "My first info log using default logger";
   return 0;
}
```
> 2017-07-30 10:15:42,642 INFO [default] My first info log using default logger

[GoBack!](../README.md)
