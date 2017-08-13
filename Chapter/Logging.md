# 记录日志
## 最简方式
下面两种是Easylogging最简单的记录日志方式：
- **LOG(LEVEL)**
- **CLOG(LEVEL, logger ID)**

**注:**  
**logger ID**是用于标识记录者的(即，名称)，具体使用参见下面的`多用户记录日志 `
**LOG(LEVEL)**其实是**CLOG(LEVEL, default)**  
如果只想在`Debug`的时候记录日志可以使用**DLOG(LEVEL)/DCLOG(LEVEL, logger ID)**  
例子：
```c++
LOG(INFO) << "This is info log";
CLOG(ERROR, "performance") << "This is info log using performance logger";
```
> 2017-08-09 21:49:31,968 INFO [default] This is info log  
> 2017-08-09 21:49:31,995 ERROR This is info log using performance logger

## 多用户记录日志
在写日志的时候可以用`logger ID`区分不同的记录者。
### 注册logger
如果使用CLOG(LEVEL, logger ID)写日志的时候，logger没有注册。那么日志会提示：  
>2017-08-09 22:00:45,703 DEBUG [default] [xiao@DESKTOP-T7H4HJF] [int __cdecl main(int,char *[])] [d:\tmp\easylog\easylog\Դ.cpp:24] Logger [Xiao] is not registered yet!  

使用下面的函数注册logger:  
```
static Logger* getLogger(const std::string& identity, bool registerIfNotAvailable = true);
```
**注:**  
获取创建日志记录者和获取日志记录者是用同一个函数  
registerIfNotAvailable为`true`,如果identiy不存在，则会创建一个Logger并返回指针；registerIfNotAvailable为`false`, 如果identiy不存在，则会返回一个空指针。 

### 打开多用户记录功能
默认情况下多用户记录日志是不打开的，要将标志位`el::LoggingFlag::MultiLoggerSupport`设置为**true**  
**推荐：**给给特定用户定义一个宏来记录日志   
例子：  
```c++
#include "easylogging++.h"
INITIALIZE_EASYLOGGINGPP
int main(void) {

    el::Loggers::addFlag(el::LoggingFlag::MultiLoggerSupport); // 使能多用户模式
    el::Loggers::getLogger("network"); // 注册'network' logger
    
    CLOG(INFO, "default", "network") << "My first log message that writes with network and default loggers";

    // Another way of doing this may be
    #define _LOGGER "default", "network"
    CLOG(INFO, _LOGGER) << "This is done by _LOGGER";

    // More practical way of doing this 推荐这个方法
    #define NETWORK_LOG(LEVEL) CLOG(LEVEL, _LOGGER)
    NETWORK_LOG(INFO) << "This is achieved by NETWORK_LOG macro";
    
return 0;
}
```
> 2017-08-09 22:29:09,045 INFO [default] My first log message that writes with network and default loggers  
> 2017-08-09 22:29:09,051 INFO [network] My first log message that writes with network and default loggers  
> 2017-08-09 22:29:09,056 INFO [default] This is done by _LOGGER  
> 2017-08-09 22:29:09,059 INFO [network] This is done by _LOGGER  
> 2017-08-09 22:29:09,062 INFO [default] This is achieved by NETWORK_LOG macro  
> 2017-08-09 22:29:09,066 INFO [network] This is achieved by NETWORK_LOG macro  

## 条件写日志
Easylogging提供满足特定条件情况下才写日志功能。
### 满足条件写日志
当`condition`为**true**的时候会写日志
- **LOG_IF(condition, LEVEL)**
- **CLOG_IF(condition, LEVEL, logger ID)**
例子： 
```c++
LOG_IF(condition, INFO) << "Logged if condition is true";
LOG_IF(false, WARNING) << "Never logged";
CLOG_IF(true, INFO, "performance") << "Always logged (performance logger)"
```

### 每执行n次写日志
每执行n次(**LOG**或**CLOG**)写一次日志，记录之后会重新计数。  
- **LOG_EVERY_N(n, LEVEL)**
- **CLOG_EVERY_N(n, LEVEL, logger ID)**

### 执行n次之后写日志
在执行到n次之后开始记录日志。  
- **LOG_AFTER_N(n, LEVEL)**
- **CLOG_AFTER_N(n, LEVEL, logger ID)**
### 写n次日志
只写n次日志，之后不再记录日志。
- **LOG_N_TIMES(n, LEVEL)**
- **CLOG_N_TIMES(n, LEVEL, logger ID)**

### 例子
```c++
for (int i = 1; i <= 10; ++i) {
   LOG_EVERY_N(2, INFO) << "Logged every second iter";
}
// 5 logs written; 2, 4, 6, 7, 10

for (int i = 1; i <= 10; ++i) {
   LOG_AFTER_N(2, INFO) << "Log after 2 hits; " << i;
}
// 8 logs written; 3, 4, 5, 6, 7, 8, 9, 10

for (int i = 1; i <= 100; ++i) {
   LOG_N_TIMES(3, INFO) << "Log only 3 times; " << i;
}
// 3 logs writter; 1, 2, 3
```

[GoBack !](../README.md)
