hard realtime:在ddl前完成
soft realtime:可以延迟,但是不能太久

race condition:多个进程同时访问和操作资源,最后的资源值取决于那个进程最后结束,为了防止竞争关系,并发程序必须同步synchronized
synchronziation:使用原子操作保证进程的合作 atomic operation是同步的基础
mutual exclusion(互斥):一个时间只能有一个在执行
critical section:临界区 一个代码区只有一个进程能够执行 是互斥的结果
多个进程同时访问一个资源,操作这个资源的代码就是critical section,一个访问其他不能访问.
semaphores:信号量P()-1,V()+1,必须是原子操作
P():while(s<=0);s-- 这是一种自旋锁(忙等待)
V():s++;
可以使用等待队列来把进程阻塞block或者唤醒wakeup
```c++
Wait (S){
	value--;
	if (value < 0) {
		add this process to waiting queue
		block();
	}
}

Signal (S){
	value++;
	if (value <= 0) {	
		remove a process P from the waiting queue
		wakeup(P);
	}
}
```
有两种方法:Binary信号量,只能是0/1
counting信号量:无限制