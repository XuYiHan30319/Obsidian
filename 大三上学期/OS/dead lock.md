# 4个必要条件
- Mutual exclusion
- Hold and wait
- No  preemption
- Circual wait

# 避免方法
- 鸵鸟法:假装没发生
- 恢复法
	- 检测
	- 恢复
- 永远不会发生
	- prevention
	- avoidance


## 死锁prevention
保证至少有一个不满足,通过资源分配方式来限制
Mutual Exclusion:不请求共享资源或者花不完 无限的资源
Hold And Wait:只能拿一个资源或者一次全部申请
No Preemption:如果当前申请的资源无法申请,那么释放全部资源
circular wait:排个序,挨个给,每次给全部就不会死锁

## 死锁avoidance
每个process声明最大的资源使用量,然后动态检查资源是否可用,保证没有循环等待,看看是不是safe state
![[Pasted image 20240107131102.png]]
银行家算法:分配再检查
安全性算法:直接检查
# 死锁检测与恢复

随便杀一个






