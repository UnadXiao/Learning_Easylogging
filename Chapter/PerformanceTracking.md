# 性能跟踪
8.91之前版本使用性能跟踪需要定义许多宏，之后新版本做了很多优化。只需要下面三个宏就可进行性能跟踪：  
- **TIMED_FUNC(obj-name)**
- **TIMED_SCOPE(obj-name, block-name)**
- **TIMED_BLOCK(obj-name, block-name)**  
**注：**  
**obj-name**是起的一个变量名称，用于记录时间用  
**block-name**是跟踪代码块运行时间的一个名称，会写入日志  
其实**TIMED_FUNC()**就是**TIMED_SCOPE()**`block-name`等于函数名的特例  
默认性能跟踪是**INFO**级别日志，如果要修改为其他级别，需要手动修改头文件`easylogging++.h`中的`el::base::PerformanceTracker`

**使能**  
**ELPP_DISABLE_PERFORMANCE_TRACKING**	Disables performance tracking
## 计算函数执行时间

**TIMED_FUNC(obj-name)**宏用于计算函数运行时间。在函数的开头使用，在离开这个函数的时候，会自动记录进出所消耗的时间。  
例如：  
```c++
void performHeavyTask(int iter) {
   TIMED_FUNC(timerObj);
   // Some initializations
   // Some more heavy tasks
   Sleep(500);
}
```
> 06:22:31,460 INFO Executed [void performHeavyTask(int)] in [501 ms]  

## 计算代码块执行时间
**TIMED_SCOPE(obj-name, block-name)**宏用于计算代码块的执行时间。在离开代码块的作用域的时候，会自动计算所消耗时间。  
例子一，使用`{...}`将需要计算代码块包含起来：  
```c++
{
    TIMED_SCOPE(timer, "my-block");
    for (int i = 0; i <= 500; ++i) {
        LOG(INFO) << "This is iter " << i;
    }
} 
LOG(INFO) << "By now, you should get performance result of above scope";
```
> 2017-08-10 18:35:21,588 INFO [default] This is iter 498  
2017-08-10 18:35:21,593 INFO [default] This is iter 499  
2017-08-10 18:35:21,599 INFO [default] This is iter 500  
2017-08-10 18:35:21,604 TRACE Executed [my-block] in [2 seconds]  
2017-08-10 18:35:21,610 INFO [default] By now, you should get performance result of above scope  

例子二，计算每次循环的执行时间：  
```c++
while (iter-- > 0) {
   TIMED_SCOPE(timerBlkObj, "heavy-iter");
   // Perform some heavy task in each iter
   Sleep(10000);
}
```
> ...  
06:22:31,429 INFO Executed [heavy-iter] in [10 ms]  
06:22:31,440 INFO Executed [heavy-iter] in [10 ms]  
06:22:31,450 INFO Executed [heavy-iter] in [10 ms]  
06:22:31,460 INFO Executed [heavy-iter] in [10 ms]  

## CheckPoint
**TIMED_FUNC**，**TIMED_SCOPE**和**TIMED_BLOCK**三个宏会调用`checkpoint`方法记录时间。  
Easylogging提供了下面这些宏可以手动调用`checkpoint`方法记录时间：  
- **PERFORMANCE_CHECKPOINT(timed-block-obj-name)**
- **PERFORMANCE_CHECKPOINT_WITH_ID(timed-block-obj-name, id)**

**注：**  
每次调用上面的宏，记录的时间范围是：从**TIMED_FUNC/SCOPE/BLOCK**开始到**PERFORMANCE_CHECKPOINT**结束。  

例子：  
```c++
void performHeavyTask(int iter) {
   TIMED_FUNC(timerObj);
   // Some initializations
   // Some more heavy tasks
   usleep(5000);
   while (iter-- > 0) {
       TIMED_SCOPE(timerBlkObj, "heavy-iter");
       // Perform some heavy task in each iter
       // Notice following sleep varies with each iter
       usleep(iter * 1000);
       if (iter % 3) {
           PERFORMANCE_CHECKPOINT(timerBlkObj);
       }
   }
}
```
> 06:33:07,558 INFO Executed [heavy-iter] in [9 ms]  
06:33:07,566 INFO Performance checkpoint for block [heavy-iter] : [8 ms]  
06:33:07,566 INFO Executed [heavy-iter] in [8 ms]  
06:33:07,573 INFO Performance checkpoint for block [heavy-iter] : [7 ms]  
06:33:07,573 INFO Executed [heavy-iter] in [7 ms]  
06:33:07,579 INFO Executed [heavy-iter] in [6 ms]  
06:33:07,584 INFO Performance checkpoint for block [heavy-iter] : [5 ms]  
06:33:07,584 INFO Executed [heavy-iter] in [5 ms]  
06:33:07,589 INFO Performance checkpoint for block [heavy-iter] : [4 ms]  
06:33:07,589 INFO Executed [heavy-iter] in [4 ms]  
06:33:07,592 INFO Executed [heavy-iter] in [3 ms]  
06:33:07,594 INFO Performance checkpoint for block [heavy-iter] : [2 ms]  
06:33:07,594 INFO Executed [heavy-iter] in [2 ms]  
06:33:07,595 INFO Performance checkpoint for block [heavy-iter] : [1 ms]  
06:33:07,595 INFO Executed [heavy-iter] in [1 ms]  
06:33:07,595 INFO Executed [heavy-iter] in [0 ms]  
06:33:07,595 INFO Executed [void performHeavyTask(int)] in [51 ms]  

[GoBack !](../README.md)
