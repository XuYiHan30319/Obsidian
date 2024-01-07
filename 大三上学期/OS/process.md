资源对与多个程序都可用,cpu在多个程序之间切换
## process是什么
process就是正在执行的程序,是一个电脑中正在执行的程序的实体,使用process control block(PCB)来表示
process 的状态:![[Pasted image 20240107093439.png]]
PCB就是存储每个process信息的逻辑块
## process调度
process的调度可以被分为IO-bound process和CPU-bound process,io bound就是io上使用的时间更多
对于单处理器的程序,同时只能处理一个任务,剩下的任务必须等待cpu空闲
- 长期调度Long term/job scheduler:选择任务进入ready队列
- 短期调度:Short term scheduler/cpu scheduler:选择下一个执行的进程
- 中期调度:medium-term scheduler/swapping 将process从memory中移出或移入改变并发度
![[Pasted image 20240107100455.png]]
切换process 的过程叫做context-switch
进程同步,死锁,通信,创建和删除,挂起和恢复
一个进程可以创建子进程,这些进程共享资源(儿子共享父亲但是反之不行)并且并发执行
## 进程通信
