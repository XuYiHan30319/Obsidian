现代os扩展了thread概念,能让一个进程同时执行多个线程,process依然只拥有一个地址,但是这个地址范围内可以创建新的线程.线程可以访问process和其他线程
线程的生命周期和process一样,创建线程比创建process cheaper
- 更高效
- 资源共享
- 经济:创建和销毁更快
- 反应:部分被阻塞依然再跑

进程拥有完整的资源,线程减少时间空间开销,线程通信不走kernel,切换时间更短

## 多线程模型
用户等级的线程user-level threads:快,便宜,但是如果kernel单线程,System call能够阻塞zheng'ge're
内核支持的线程kernel supported threads,能够单独的运行或者阻塞,一个进程可能有多个不同的进程,有点expensive