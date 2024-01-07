![[Pasted image 20240107153426.png]]![[Pasted image 20240107153437.png]]
磁盘的基础存储单元是sector,
disk performance:
- seek time:position the head/arm over the proper track (into proper cylinder):找到正确的扇面
- rotational latency:wait for the desired sector to rotate under the read/write head:旋转到正确的位置
- transfer time:transfer a block of bits (sector) under the read-write head:传送bit
- disk latency延时:Queueing Time + Controller time+disk time
- average position time:seek time+半圈的时间


# Disk Scheduling
- FCFS :先来先服务
- SSTF shortest seek time first:离当前最短的优先
- SCAN:跑到头再回来
- CSCAN:跑到头再回来,回来的路上不干活
- LOOK:跑到最上面的一个操作位置
- CLOOK:跑到最上面的一个操作位置再回来,回来的路上不干活

 