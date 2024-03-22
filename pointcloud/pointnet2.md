>[PointNet++论文解析与代码详解(含特征维度变化框图和代码注释) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/164880749)

对于点云分割部分，主要有三个：
1. set abstraction部分，用于迭代从而得到局部点云的结构
2. FP上采样部分，用于进行分割

![[Pasted image 20240321164951.png]]
## SA部分
Sampling layer: 主要是针对输入点进行采样,在这些点中选出若干个中心点(采集方法代码中采用的是FPS的方法.)  
 Grouping layer:利用上一步得到的中心点将点集划分成若干个区域,可以设置不同的圆形半径以及每个圆形区域中最多的点的个数;  
Pointnet layer:对上述得到的每个区域进行编码,获得特征向量,将每个区域中的特征向量叠加在区域中心点的特征通道上,区域中心点的最开始特征通道为3(x,y,z三个坐标值.)
每一组提取层的输入是B\*N(\d+C),其中B是batch_size的大小,N是输入点的数量,d是坐标维度,C是特征维度,中心点集的维度是N'\*d,输出是B\*N'\*(d+C'),其中B是batch_size的大小,N'是输出点的数量,d是坐标维度,C'是特征维度.这里的B是不变的,N'即是采样后的中心点个数.C'是采样的特征维度.
首先是sampling layer，使用最远点采样算法选择N个点
然后是Grouping layer，使用ball query生成N'个区域，这里有两个变量,一个是球形区域的半径,一个是区域中点的数量K.，但是PointNet层能够去把复杂数量的点转成一个固定局部区域特征向量.Ball query可以会找到球形区域内的所有点,这里的点的上限是K.这里也可以使用KNN的方法去找固定邻域的点,相对而言,使用Ball query可以更多的专注局部区域特征,更适用于局部的模式识别任务(例如semantic point labeling)
pointnet layer,输入N\*k\*(d+c),输出N\*(d+c)
4.非均匀采样密度下鲁棒的特征学习
由于不同区域的密度不同，稠密的数据学习特征不适用于稀疏区域，所以提出了MSG多尺度分组和MRG多分辨率分组
![[Pasted image 20240321170151.png]]
MSG就是用不同尺度的大小，然后拼接起来
MRG就是,左侧的特征是通过将底层每个子区域中经过SAmodules获得的带有局部区域特征的中心点通过PointNet获得的特征,右侧是直接在底层局部区域的原始点上直接使用PointNet获得的特征,然后将两个特征进行级联.
DP：随机丢弃数据集中的点云
![[Pasted image 20240321194626.png]]
![[Pasted image 20240321194819.png]]
![[Pasted image 20240321200311.png]]
> xyz-->2\*1024\*3,其中2表示batch_size的大小,1024表示输入的点云的点数,3表示的是点的特征维度,也就是每个点是有xyz三个坐标值.  
new_xyz_-->_2\*512\*3,其中2还是batch___size大小,512表示的第一次要采样的中心点的个数,采样出来512个中心点,每个中心点的维度为3.  
grouped_xyz--> 2*3*512*16,其中3表示的是点的维度xyz,512表示的512个采样出来的中心点,16是以每个中心点按照设置的半径(0.1),设置的球内点的数量的上限,我们在上面的代码中设置的以此为16,32,128.  
new_feature-->2*64*512*16,对应上面,将表示点云维度的3进行特征提取,提取后的点云特征维度为64.细节可以看到,上面点云的SharedMLP(layer0)中的第一个二维卷积输入维度是3,然后对应的维度变化是3-->32-->32-->64.其他的半径的特征同理  
最后经过max pooling之后,最后输出的维度为2*64*512.也与论文中所说的经过PointNet layer提取后得到的维度是对应的.512表示的是512个中心点,64表示的是每个中心点的特征维度.  
最后将三个不同尺度获得的特征进行级联,即MSG的方式,获得的特征是2*320*512,而对应的采样点的xyz的那部分维度为2*512*3,送入下一层的SA_module的特征维度是2*512*(320+3).512对应的是N',3对应的是d,320对应的是C'.

上面是MSG的方式。接下来我们再使用上采样得到结果
![[Pasted image 20240321200414.png]]