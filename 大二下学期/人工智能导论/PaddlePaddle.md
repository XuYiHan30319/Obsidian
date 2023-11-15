```python
异步数据读取使用飞桨实现异步数据读取非常简单，只需要两个步骤：
1.  构建一个继承paddle.io.Dataset类的数据读取器。
2.  通过paddle.io.DataLoader创建异步数据读取的迭代器。
首先，我们创建定义一个paddle.io.Dataset类，使用随机函数生成一个数据读取器，代码如下：
import numpy as np
from paddle.io import Dataset
# 构建一个类，继承paddle.io.Dataset，创建数据读取器
class RandomDataset(Dataset):
	def __init__(self, num_samples):
		# 样本数量
		self.num_samples = num_samples

	def __getitem__(self, idx):
	# 随机产生数据和label
		image = np.random.random([784]).astype('float32')
		label = np.random.randint(0, 9, (1, )).astype('float32')
		return image, label

	def __len__(self):
		# 返回样本总数量
		return self.num_samples
# 测试数据读取器
dataset = RandomDataset(10)
for i in range(len(dataset)):
	print(dataset[i])

loader = paddle.io.DataLoader(dataset, batch_size=3, shuffle=True, drop_last=True, num_workers=2)
for i, data in enumerate(loader()):
	images, labels = data[0], data[1]
print("batch_id: {}, 训练数据shape: {}, 标签数据shape: {}".format(i, images.shape, labels.shape))
```
显然,手写体识别并非线性的神经网络可以实现的,所以,我们需要采用其他类型的神经网络来实现
在设计多层神经网络的时候,我们的class与单层的类似
```python
import paddle.nn.functional as F
from paddle.nn import Linear
# 定义多层全连接神经网络
class MNIST(paddle.nn.Layer):
	def __init__(self):
		super(MNIST, self).__init__()
		# 定义两层全连接隐含层，输出维度是10，当前设定隐含节点数为10，可根据任务调整
		self.fc1 = Linear(in_features=784, out_features=10,act = "sigmoid")
		self.fc2 = Linear(in_features=10, out_features=10,act="sigmoid")
		# 定义一层全连接输出层，输出维度是1
		self.fc3 = Linear(in_features=10, out_features=1,act="none")
	# 定义网络的前向计算，隐含层激活函数为sigmoid，输出层不使用激活函数
	def forward(self, inputs):
		# inputs = paddle.reshape(inputs, [inputs.shape[0], 784])
		outputs1 = self.fc1(inputs)
		# outputs1 = F.sigmoid(outputs1)
		outputs2 = self.fc2(outputs1)
		# outputs2 = F.sigmoid(outputs2)
		outputs_final = self.fc3(outputs2)
		return outputs_final
```
我们在MNIST中,首先继承了paddle.nn.Layer的深度神经网络,然后在构造函数中定义了三层,分别为两个全连接层和一个输出层,其中我们假定他还是个回归问题,输出层的个数是1.在forward函数中我们的设计也类似,只不过我们将每一层的结果都用了sigmoid函数进行激活,然后再将上一层的结果传入下一层.
# 卷积神经网络
上面的全连接神经网络效果果然比单层的好(没测过),但是使用全连接的方式会导致我们失去像素的空间位置关系.所以我们采用更好的卷积神经网络,因为他直接对原图进行处理,更好的保持了像素之间的空间关系,方便计算机对于他的理解.卷积神经网络拥有卷积层和池化层两层.卷积层用于提取更为高端的特征,而池化层用于对卷积的结果过滤突出池化特征
局部视野(只需要一点点的视觉区域就可以知道他是猫)和权值共享

# 我们应该使用什么损失函数?
在波士顿房价问题和MNIST手写体识别问题中,我们发现这两个问题有很大的区别,因此,使用均方差损失函数并不能很好的度量损失.
如果我们使用softmax函数,就能极大化
交叉熵函数
# 学习率
-   **学习率不是越小越好**。学习率越小，损失函数的变化速度越慢，意味着我们需要花费更长的时间进行收敛，如 **图2** 左图所示。并且可能陷入局部最大值
-   **学习率不是越大越好**。只根据总样本集中的一个批次计算梯度，抽样误差会导致计算出的梯度不是全局最优的方向，且存在波动。在接近最优解时，过大的学习率会导致参数在最优解附近震荡，损失难以收敛，如 **图2** 右图所示。
![[Pasted image 20230515143714.png]]
此在训练时可以尝试调小或调大，通过观察Loss下降的情况判断合理的学习率，设置学习率的代码如下所示。
我们暂时使用sgd梯度下降法
```python
opt = paddle.optimizer.SGD(learning_rate=0.001,parameters=model.parameters())
```

# 加速计算
```python
#在使用GPU机器时，可以将use_gpu变量设置成True
use_gpu = True
paddle.set_device('gpu:0') if use_gpu else paddle.set_device('cpu')
```

# 模型测试
# 加入校验或测试，更好评价模型效果 
在训练过程中，我们会发现模型在训练样本集上的损失在不断减小。但这是否代表模型在未来的应用场景上依然有效？为了验证模型的有效性，通常将样本集合分成三份，训练集、校验集和测试集。
- **训练集** ：用于训练模型的参数，即训练过程中主要完成的工作。
- **校验集** ：用于对模型超参数的选择，比如网络结构的调整、正则化项权重的选择等。
- **测试集** ：用于模拟模型在应用后的真实效果。因为测试集没有参与任何模型优化或参数训练的工作，所以它对模型来说是完全未知的样本。在不以校验数据优化网络结构或模型超参数时，校验数据和测试数据的效果是类似的，均更真实的反映模型效果。

如下程序读取上一步训练保存的模型参数，读取校验数据集，并测试模型在校验数据集上的效果。
```python
def evaluation(model):
	print('start evaluation .......')
	# 定义预测过程
	params_file_path = 'mnist.pdparams'
	# 加载模型参数
	param_dict = paddle.load(params_file_path)
	model.load_dict(param_dict)
	model.eval()# 设置为预测模式
	eval_loader = load_data('eval')
	acc_set = []
	avg_loss_set = []
	for batch_id, data in enumerate(eval_loader()):
		images, labels = data
		images = paddle.to_tensor(images)
		labels = paddle.to_tensor(labels)
		predicts, acc = model(images, labels)
		loss = F.cross_entropy(input=predicts, label=labels)
		avg_loss = paddle.mean(loss)
		acc_set.append(float(acc.numpy()))
		avg_loss_set.append(float(avg_loss.numpy()))
		#计算多个batch的平均损失和准确率
		acc_val_mean = np.array(acc_set).mean()
		avg_loss_val_mean = np.array(avg_loss_set).mean()
		print('loss={}, acc={}'.format(avg_loss_val_mean, acc_val_mean))

	model = MNIST()	
	evaluation(model)
```

# 如何防止过拟合
通常过拟合的发生是因为数据集太少而且模型太复杂,学习到了样本数据中的一些错误数据
为了防止这种情况发生,通常我们选择的方法是使用正则化,降低模型复杂度
比如在本项目中,我们可以使用正则化
```python
opt = paddle.optimizer.SGD(learning_rate=0.01, weight_decay=paddle.regularizer.L2Decay(coeff=1e-5), parameters=model.parameters())
```
各种优化算法均可以加入正则化项，避免过拟合，参数regularization_coeff调节正则化项的权重,具体的正则化代码如上所示,我也不知道这是什么参数.

# 可视化分析
通常我们使用matplotbil库来实现数据的可视化
我们将训练的批次作为x轴,损失函数作为y轴,然后我们先声明两个队列来记录下对应的批次号和损失函数.训练结束之后将损失函数放入plt中进行可视化

```python
import matplotlib.pyplot as plt

plt....
```
# 模型中断训练并且重新开始训练
