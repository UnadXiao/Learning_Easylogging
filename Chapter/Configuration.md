# 日志设置
## 日志等级
Easylogging记录日志一共分9个等级：

| Global | Trace | Debug | Fatal | Error | Warning | Info | Verbose | Unknown |
| ------ | ----- | ----- | ----- | ----- | ------ | ----- |----- | ---- |
| **概念级**  |      |        |       |       |        |       |     | **概念级** |
**注：**  
`概念级`只能用于设置，没有对应写日志的等级。
默认所有日志等级平行关系。如果想要分等级记录日志则需要使用标志位`LoggingFlag::HierarchicalLogging`。

## 配置日志
Easylogging有三种设置日志方法:
- Configuration文件配置
- 使用Configuration类
- 使用内联Configuration

**注：**  
三种方法这里只介绍Configuration文件配置方法。  
另外两种方法见[Using el::Configurations Class](https://github.com/UnadXiao/easyloggingpp#using-elconfigurations-class) [Using In line Configurations](https://github.com/UnadXiao/easyloggingpp#using-in-line-configurations)

### Configuration文件格式
```json
* LEVEL:
  CONFIGURATION NAME  = "VALUE" ## Comment
  ANOTHER CONFIG NAME = "VALUE"
```
**注：**  
等级以`*`开始`：`结束。建议首先写`Global`等级，之后的等级都会继承之前的等级设置。  
`CONFIGURATION NAME`是配置名称，`VALUE`是配置值。  
`##`后是注释。  

**CONFIGURATION NAME如下：**

| Configuration Name | Type | Description |
| ------ | ----- | ----- |
| `Enabled`               |   bool   | (对应等级)日志使能。如果`Global`等级为**false**，则所有(等级)日志都不会记录|
| `To_File`               |   bool   | 将日志输出到文件                                                       |
| `To_Standard_Output`    |   bool   | 将日志输出到标准输出显示                                                |
| `Format`                |   char*  | 日志输出格式，具体见下面**日志输出格式**                                 |
| `Filename`              |   char*  | 输出log文件名                                                          |
| `Subsecond_Precision`   |   uint   | 记录日志时间的精度(位数)，设置范围(1-6)                                  |
| `Performance_Tracking`  |   bool   | 性能跟踪使能。性能跟踪一般使用`performance`logger(日志记录者)记录，不依赖于logger和等级|
| `Max_Log_File_Size`     |   size_t | 最大日志文件大小。如果日志文件大于这个值，后续内容会被截断                 |
| `Log_Flush_Threshold`   |  size_t  | Flush入日志文件的阈值(日志条数)                                         |

**例子：**
```c++
* GLOBAL:
   FORMAT               =  "%datetime %msg"
   FILENAME             =  "/tmp/logs/my.log"
   ENABLED              =  true
   TO_FILE              =  true
   TO_STANDARD_OUTPUT   =  true
   SUBSECOND_PRECISION  =  6
   PERFORMANCE_TRACKING =  true
   MAX_LOG_FILE_SIZE    =  2097152 ## 2MB - Comment starts with two hashes (##)
   LOG_FLUSH_THRESHOLD  =  100 ## Flush after every 100 logs
* DEBUG:
   FORMAT               = "%datetime{%d/%M} %func %msg"
```

### 日志输出格式

|     Specifier   |                 Replaced By                                                                 |
|-----------------|---------------------------------------------------------------------------------------------|
| `%logger`       | Logger ID，用于表示那个用户记录的(多用户记录会用到)                                             |
| `%thread`       | Thread ID                                                                                   |
| `%thread_name`  | 自定义的线程                                                                                 |
| `%level`        | 严重等级(Info, Debug, Error, Warning, Fatal, Verbose, Trace)                                 |
| `%levshort`     | 严重等级缩写版(I D, E, W, F, V, T)                                                           |
| `%vlevel`       | Verbosity等级(用于详细记录信息)                                                               |
| `%datetime`     | 日期，具体见下面**时间格式**                                                                  |
| `%user`         | 当前用户                                                                                     |
| `%host`         | 主机名                                                                                       |
| `%file`*         | (完整路径)文件名，是编译器的`__FILE__` 宏实现的                                               |
| `%fbase`*        | (不包含路径)文件名                                                                           |
| `%line`*         | 源代码中的行号，是编译器的 `__LINE__` 宏实现的                                                 |
| `%func`*         | 写日志处的函数名                                                                             |
| `%loc`*          | 源代码的文件名加行号，用`:`隔开                                                               |
| `%msg`          | 日志信息                                                                                     |
| `%`             | 转意字符                                                                                     |

### 时间格式

| Specifier | Replaced By                              |
| --------- | ---------------------------------------- |
| `%d`      | Day of month (zero-padded)               |
| `%a`      | Day of the week - short (Mon, Tue, Wed, Thu, Fri, Sat, Sun) |
| `%A`      | Day of the week - long (Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday) |
| `%M`      | Month (zero-padded)                      |
| `%b`      | Month - short (Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec) |
| `%B`      | Month - Long (January, February, March, April, May, June, July, August, September, October, November, December) |
| `%y`      | Year - Two digit (13, 14 etc)            |
| `%Y`      | Year - Four digit (2013, 2014 etc)       |
| `%h`      | Hour (12-hour format)                    |
| `%H`      | Hour (24-hour format)                    |
| `%m`      | Minute (zero-padded)                     |
| `%s`      | Second (zero-padded)                     |
| `%g`      | Subsecond part (precision is configured by ConfigurationType::SubsecondPrecision) |
| `%F`      | AM/PM designation                        |
| `%`       | Escape character                         |

### 例子
```json
* GLOBAL:
   FORMAT               =  "%datetime %msg"
   FILENAME             =  "/tmp/logs/my.log"
   ENABLED              =  true
   TO_FILE              =  true
   TO_STANDARD_OUTPUT   =  true
   SUBSECOND_PRECISION  =  6
   PERFORMANCE_TRACKING =  true
   MAX_LOG_FILE_SIZE    =  2097152 ## 2MB - Comment starts with two hashes (##)
   LOG_FLUSH_THRESHOLD  =  100 ## Flush after every 100 logs
* DEBUG:
   FORMAT               = "%datetime{%d/%M} %func %msg"
```

[GoBack!](../README.md)
