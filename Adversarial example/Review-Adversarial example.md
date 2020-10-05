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

作者通过一种以有效的方式（通过优化）遍历网络表示的流形，并在输入空间中找到对抗样本的方法 

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190316105140127.PNG) 

但由于上式是一个NP-H问题，所以作者通过下式近似优化，并通过box-constrainted L-BFGS方法进行求解

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190316105731767.PNG) 



Reference： https://www.cnblogs.com/lucifer1997/p/12771805.html 

​					   https://blog.csdn.net/qq_43205738/article/details/84575387 

L-BFGS:  https://blog.csdn.net/u012285175/article/details/77866878 

hard negative mining:   https://blog.csdn.net/u012285175/article/details/77866878 



# Explaining and Harnessing Adversarial Examples (ICLR 2015)

