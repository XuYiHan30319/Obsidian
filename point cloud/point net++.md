## FPS最远点采样
在二维图像中经常下采样得到更小的图片,而在点云中我们也有类似算法
首先我们随机选一个点放入集合中,然后再每一轮迭代中,选取其他点到目前集合中所有点的距离的最小值最大的那个点放入集合中从而得到新的集合.
```python
import open3d as o3d 
import numpy as np 
def fps(points, num_samples): 
	pcd = o3d.geometry.PointCloud() 
	pcd.points = o3d.utility.Vector3dVector(points)
	sampled_indices = o3d.geometry.PointCloud.uniform_down_sample(pcd, num_samples) 
	return np.asarray(sampled_indices)
```