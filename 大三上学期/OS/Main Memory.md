程序被放进内存中才能跑
内存管理:
• Keeping track of which parts of memory are currently being used and by whom
• Deciding which processes (or parts thereof) and data to move into and out of memory
• Allocating and deallocating memory space as needed
物理地址:cpu产生的
逻辑地址:内存单元看见的
只有在执行地址绑定方案的时候才有区别
MMU:映射逻辑地址到物理地址,用户程序永远看不到物理地址,mmu就是逻辑地址+基地址

# 连续内存分配
每个进程都分配了一个内存分区
## 固定分区
可以固定大小也可以不一样,本来就切好的块
最小化外碎片问题:当分配之后每个分区多出来的内存叫做internal fragmentation
固定大小:如果有空闲的就可以分配,分配那个无所谓,如果有分区被占了,选择一个交换出去,
非固定大小:每个任务都分配到最小可以满足的分区.每个size大小的块都有一个任务队列,任务进入队列等待分配,会产生内碎片
![[Pasted image 20240107140556.png]]
![[Pasted image 20240107140601.png]]
![[Pasted image 20240107140606.png]]
![[Pasted image 20240107140612.png]]
# variable partitioning
当任务到了,从洞里找一个足够大的内存给他
best-fit最佳适配:找一个最小的给他,会留下一点小碎片,需要进行内存压缩Memory compaction
worst-fit:最差适配:分配一个最大的,
first-fit首次适配:从开头找一个足够大的
next-fit临近适配:从上一次的地址开始找
![[Pasted image 20240107141040.png]]![[Pasted image 20240107141048.png]]
内碎片internal fragmentation:分配的内存可能比需要的打一点,多出来的那点内存无法使用
external fragmentation外碎片:总的够,但是单个不够
# compaction压缩
外部碎片可以被压缩
![[Pasted image 20240107141547.png]]
![[Pasted image 20240107141554.png]]
需要搬运程序和数据,修改基地址(process limited长度)
# Paging
paging就是fixed partitioning但是更小
- 逻辑地址被分为页
- 物理地址被分为帧

Paging Table页表:页和帧之间的映射关系
![[Pasted image 20240107142453.png]]
虚拟内存:可以任意大小
逻辑地址被分为两块:一个是页面号page number一个是页面偏移page offset,是相对于当前页的偏移
![[Pasted image 20240107142916.png]]
逻辑地址0对应的地址为4096\*2-1+1
当块没有在内存中的时候会触发一个中断,然后去找到需要的块
Page Fault:缺页,如果没空的内存了就swap!页面置换算法page replacement algorithm
![[Pasted image 20240107143912.png]]
![[Pasted image 20240107143917.png]]
提升访问速度:提升访问速度,降低page fault
- TLB translation lookaside buffer翻译后备缓冲器
- 