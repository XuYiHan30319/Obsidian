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
可以固定大小也可以不一样
最小化外碎片问题:当分配之后每个分区多出来的内存叫做internal fragmentation
固定大小:如果有空闲的就可以分配,分配那个无所谓,如果有分区被占了,选择一个交换出去,
非固定大小:每个任务都分配到最小可以满足的分区.每个size大小的块都有一个任务队列,任务进入队列等待分配,会产生内碎片