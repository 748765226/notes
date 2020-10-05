# Intriguing properties of neural networks

# (ICLR 2014) 

 But as the resulting computation is automatically discovered by **backpropagation via supervised learning**, it can be **difficult to interpret** and can have **counter-intuitive properties**. 

由于神经网络的模型是通过**监督学习的反向传播**由计算机自主学得，这使得网络**难以解释**且存在**反直觉特性**——网络模型就像是一个*黑盒子*，你无法准确地分析其内部的详细情况，或者说无法得知网络的某种性质是由于其内部的那一部分所引起，这使得网络存在可以攻击的“*弱点* ”。 

The first property is concerned with the semantic meaning of individual units… Generally, it seems that it is the entire **space of activations**, rather than the individual units, that contains the bulk of the **semantic information**. 

 第一个特性是关于单个神经元的语义信息。作者发现，**语义信息**并非单独存在于某个神经元中，而是分布在整个**激活空间**。 

The second property is concerned with the **stability** of neural networks with respect to small perturbations to their inputs. However, we find that applying an imperceptible **non-random perturbation** to a test image, it is possible to **arbitrarily** change the network’s prediction 

第二个特性是关于神经网络对于输入受到微小扰动时的**稳定性**。作者发现对测试图片进行**非随机扰动**（人眼很难察觉扰动前后的测试图像的变化的微小扰动）时，可能改变网络的预测结果（这种改变并非随机的或者说偶然的，原文用的介词是**arbitrarily–武断地**）。作者通过最大化网络预测误差的方式对输入图片进行扰动从而产生*对抗样本*（*adversarial examples*） 

Yet, we found that **adversarial examples** are **relatively robust**, and are shared by neural networks with varied number of layers, activations or trained on different subsets of the training data. That is, if we use one neural net to generate a set of adversarial examples, we find that these examples are still **statistically** hard for another neural network even when it was trained with **different hyperparameters** or, most surprisingly, when it was trained on a different set of examples. 

作者发现，产生的**对抗样本**具有**相对鲁棒性**。用某个网络产生的对抗样本，对于其他网络也是同样（**统计学上的**）难以准确预测的（即可能存在能准确预测的例外，但根据统计规律来看，这种概率比较低），哪怕其他网络的训练是不一样的超参数甚至是在不一样的训练集上训练的。——这意味着对抗样本并不是像随机噪声那样的干扰，而是这类神经网络具有非直觉特性（ nonintuitive characteristics）和内在盲点（ intrinsic blind spots）。 

**关注对抗样本，作者有两个基本的假设：** 

1.is assumed that is possible for the output unit to assign nonsignificant (and, presumably, non-epsilon) probabilities to regions of the input space that contain no training examples in their vicinity.

2.The adversarial examples represent low-probability (high-dimensional) “pockets” in the manifold, which are hard to efficiently find by simply randomly sampling the input around a given example.

第一个假设认为没有训练过的样本分布在输入空间附近（对抗样本与输入图像十分接近），第二个假设认为对抗样本的产生是低概率事件。据此我们可以推断，由于对抗样本是低概率事件，在训练集和测试集中都很少，而模型恰好只学到非对抗样本的特征，所以当模型遇到与输入图像看起来“无异”的对抗样本时就会表现得非常脆弱。这其实有点“过拟合”的味道

作者寻找对抗样本是一个逐步优化的过程：一方面，我们需要确保添加的扰动尽可能小，以致肉眼无法察觉；另一方面，需要确保模型把对抗样本分类错误 。给出以下的目标函数

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190316105140127.PNG) 

但由于上式是一个NP-H问题，所以作者通过下式近似优化，并通过box-constrainted L-BFGS方法进行求解

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190316105731767.PNG) 

### 对抗样本有趣的现象

1. 对于论文中提到的所有网络结构（non-convolutional network, AlexNet, QuocNet），都能用上述方法生成对抗样本；
2. 对抗样本具有跨模型的泛化能力：在A模型上产生的对抗样本，有很大一部分在B模型（和A模型结构相同，超参数不同）上也有效（也能使B模型错误分类）;
3. 对抗样本具有跨数据集的泛化能力：在D1数据集训练得到的模型上产生的对抗样本，在D2数据集训练得到的模型上也有效，D1和D2属于不同的子集，两个模型是结构完全不同的模型。

#### **产生原因：** Szegedy (塞格德)等人认为是神经网络的高度非线性性质导致对抗样本的存在 



Reference： https://www.jianshu.com/p/9f58542d982a （重点)

​     				  https://www.cnblogs.com/lucifer1997/p/12771805.html 

​					   https://blog.csdn.net/qq_43205738/article/details/84575387 

L-BFGS:  https://blog.csdn.net/u012285175/article/details/77866878 

hard negative mining:   https://blog.csdn.net/u012285175/article/details/77866878 



# Explaining and Harnessing Adversarial Examples (ICLR 2015)

##### **观点：Goodfellow则认为，神经网络在高维空间的线性行为才是导致对抗样本存在的真正原因** 

 https://blog.csdn.net/u014380165/article/details/90723948 