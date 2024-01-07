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
Mutual Exclusion:不请求共享资源或者花不完 无限的资源
Hold And Wait:只能拿一个资源或者一次全部申请
Preemption:如果当前申请的资源无法申请,那么释放全部资源
circular wait:排个序,挨个给,m