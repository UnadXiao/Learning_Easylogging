# Easylogging使用简单介绍
## Introduction
个人的Easylogging的学习总结，并非按照英文原文翻译，所有对错误都不负责。

> 2017.08.07辛苦写了好几天的EasyLogging学习笔记因为Typora的Bug，清空了？！！！！  
故。。。有了这个仓库  
[英文原版地址](https://github.com/muflihun/easyloggingpp/blob/master/README.md)

## Catalogue
- [快速开始](./Chapter/QuickStart.md)
   - [下载源代码](./Chapter/QuickStart.md#%E4%B8%8B%E8%BD%BD%E6%BA%90%E4%BB%A3%E7%A0%81)
   - [配置](./Chapter/QuickStart.md#%E9%85%8D%E7%BD%AE)
   - [最简例子](./Chapter/QuickStart.md#%E6%9C%80%E7%AE%80%E4%BE%8B%E5%AD%90)
- [日志设置](./Chapter/Configuration.md)
   - [日志等级](./Chapter/Configuration.md#%E6%97%A5%E5%BF%97%E7%AD%89%E7%BA%A7)
   - [配置日志](./Chapter/Configuration.md#%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97)
   - [Configuration文件格式](./Chapter/Configuration.md#configuration%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F)
   - [日志输出格式](./Chapter/Configuration.md#%E6%97%A5%E5%BF%97%E8%BE%93%E5%87%BA%E6%A0%BC%E5%BC%8F)
   - [时间格式](./Chapter/Configuration.md#%E6%97%B6%E9%97%B4%E6%A0%BC%E5%BC%8F)
- [记录日志](./Chapter/Logging.md)
   - [最简方式](./Chapter/Logging.md#%E6%9C%80%E7%AE%80%E6%96%B9%E5%BC%8F)
   - [多用户记录日志](./Chapter/Logging.md#%E5%A4%9A%E7%94%A8%E6%88%B7%E8%AE%B0%E5%BD%95%E6%97%A5%E5%BF%97)
   - [条件写日志](./Chapter/Logging.md#%E6%9D%A1%E4%BB%B6%E5%86%99%E6%97%A5%E5%BF%97)
- [性能跟踪](./Chapter/PerformanceTracking.md)
   - [计算函数执行时间](./Chapter/PerformanceTracking.md#%E8%AE%A1%E7%AE%97%E5%87%BD%E6%95%B0%E6%89%A7%E8%A1%8C%E6%97%B6%E9%97%B4)
   - [计算代码块执行时间](/Chapter/PerformanceTracking.md#%E8%AE%A1%E7%AE%97%E4%BB%A3%E7%A0%81%E5%9D%97%E6%89%A7%E8%A1%8C%E6%97%B6%E9%97%B4)
   - [CheckPoint](./Chapter/PerformanceTracking.md#checkpoint)

## Version
创建时间：2017.08.07

版本号：1.3v

修改记录：

- [2017.08.07 xiaozp] 创建本文档
- [2017.08.08 xiaozp] 添加日志设置部分内容
- [2017.08.09 xiaozp] 添加纪录日志部分内容
- [2017.08.10 xiaozp] 添加性能跟踪部分内容
