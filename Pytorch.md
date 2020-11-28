# **Pytorch**

**https://zhuanlan.zhihu.com/p/250649565**

#### 1 Tensor 

#### 	1.1 torch.FloatTensor （维度大小或者[,,,]列表 ）同理 torch.IntTensor

#### 	1.2 torch.rand(维度) 随机生成浮点数据在0~1区间均匀分布

#### 	1.3 torch.randn(维度) 随机生成的浮点数的取值满足均值为0，方差为1的正太分布

#### 	1.4 torch.range(起始，结束，步长) 浮点型 torch.arange(起始，结尾不包括) 整型

#### 	1.5 torch.zeros(维度) 浮点型，元素全部为0 torch.ones torch.empty torch.size维度

#### 	1.6 Tensor 运算 

#### 		 a=torch.tensor a.abs,a.add

##### 			1.6.1 torch.abs(tensor)  

##### 			1.6.2 torch.add(两个都是tensor，或者一个tensor一个标量)

##### 			1.6.3 torch.clamp(tensor，元素上边界，元素下边界)

##### 			1.6.4 torch.div(两个都是tensor，或者一个tensor一个标量) 相除（对应元素）

##### 			1.6.5 torch.mul(两个都是tensor，或者一个tensor一个标量) 相除（对应元素）

##### 			1.6.6 torch.pow(两个都是tensor，或者一个tensor一个标量) 求幂（对应元素）

##### 			1.6.7 torch.mm(两个都是tensor) 满足矩阵相乘要求

##### 			1.6.8 torch.mv(tensor,向量)

​			**1.6.9 torch.item() 取tensor的值**

#### 	1.7torch.autograd和Variable 　　

​	**torch.autograd包的主要功能是完成神经网络后向传播中的链式求导，手动实现链式求导的代码会给我们造     成很大的困扰，而torch.autograd包中丰富的类减少了这些不必要的麻烦。**
 **实现自动梯度功能的过程大概分为以下几步：  **

**1 通过输入的Tensor数据类型的变量在神经网络的前向传播过程中生成一张计算图** 

**2 根据这个计算图和输出结果准确计算出每个参数需要更新的梯度**  

**3 通过完成后向传播完成对参数梯度的更新**  　　

**在实践中完成自动梯度需要用到torch.autograd包中的Variable类对我们定义的Tensor数据类型变量进行封装，在封装后，计算图中的各个节点就是一个variable 对象，这样才能应用自动梯度的功能。autograd package是PyTorch中所有神经网络的核心。**

**autograd.Variable 是这个package的中心类。它打包了一个Tensor，并且支持几乎所有运算。一旦你完成了你的计算，可以调用.backward()，所有梯度就可以自动计算。** 　

**你可以使用.data属性来访问原始tensor。相对于变量的梯度值可以被积累到.grad中。 　　这里还有一个类对于自动梯度的执行是很重要的：Function（函数） 　　变量和函数是相互关联的，并且建立一个非循环图。每一个变量有一个.grad_fn属性，它可以引用一个创建了变量的函数（除了那些用户创建的变量——他们的grad_fn是空的）。** 　　

**如果想要计算导数，可以调用Variable上的.backward()。如果变量是标量（只有一个元素），你不需要为backward()确定任何参数。但是，如果它有多个元素，你需要确定grad_output参数（这是一个具有匹配形状的tensor）。**



**用X表示我们选中的节点（一个Variable对象），X.data代表Tensor数据类型 的变量，X.grad也是一个Variable对象，不过他代表的是X的梯度，在想访问梯度值的时候需要X.grad.data**



***from torch.autograd import Variable***

***x = Variable(torch.randn(batchn , inputdata) , requires_grad = False)***  

***x.data 访问原始tensor torch.randn(batchn , inputdata)，x.grad访问变量的梯度，x.grad.data 访问梯度的值***

***x.grad.data.zero_()梯度清0***



**从tensor 到numpy**

**a=torch.rand(3,5)**

**b=a.numpy()**

**从numpy到tensor**

**c=torch.from_numpy(b)**



**torch.view()只能处理contigunous内存连续的tensor 而torch.resize()没有限制。**

**torch.reshape()/torch.view() 改变形状的时候，总个数不能改变，torch.resize（）个数太多可以截取**



**r’E:\anaconda\model.pth’ # 字符串前面加r，表示的意思是禁止字符串转义**



#### 保存模型步骤 

**torch.save(model, ‘net.pth’) # 保存整个神经网络的模型结构以及参数** 

**torch.save(model, ‘net.pkl’) # 保存整个神经网络的模型结构以及参数** 

**torch.save(model.state_dict(), ‘net_params.pth’) # 只保存模型参数** 

**torch.save(model.state_dict(), ‘net_params.pkl’) # 只保存模型参数  加载模型步骤** 

**model = torch.load(‘net.pth’) # 加载整个神经网络的模型结构以及参数** 

**model = torch.load(‘net.pkl’) # 加载整个神经网络的模型结构以及参数** 

**model.load_state_dict(torch.load(‘net_params.pth’)) # 仅加载参数** 

**model.load_state_dict(torch.load(‘net_params.pkl’)) # 仅加载参数**



服务器GPU状态查询

  1） lspci | grep -i nvidia 可以查询所有nvidia显卡

  2） lspci -v -s [显卡编号] 可以查看显卡具体属性

  3） nvidia-smi 可以查看显卡的显存利用率

torch.cuda主要函数

  1) 查看是否有可用GPU、可用GPU数量： torch.cuda.is_available(), torch.cuda.device_count()

  2) 查看当前使用的GPU序号：torch.cuda.current_device()

  3) 查看指定GPU的容量、名称：

​    torch.cuda.get_device_capability(device), torch.cuda.get_device_name(device)

  4) 清空程序占用的GPU资源： torch.cuda.empty_cache()

  5) 为GPU设置随机种子：torch.cuda.manual_seed(*seed*), torch.cuda.manual_seed_all(*seed*)



