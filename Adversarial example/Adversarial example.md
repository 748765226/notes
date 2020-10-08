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



> Reference： https://www.jianshu.com/p/9f58542d982a （重点)
>
> ​     				  https://www.cnblogs.com/lucifer1997/p/12771805.html 
>
> ​					   https://blog.csdn.net/qq_43205738/article/details/84575387 
>
> L-BFGS:  https://blog.csdn.net/u012285175/article/details/77866878 
>
> hard negative mining:   https://blog.csdn.net/u012285175/article/details/77866878 



# Explaining and Harnessing Adversarial Examples (ICLR 2015)

Early attempts at explaining this phenomenon (adversarial examples) focused on **nonlinearity and overfitting**. We argue instead that the primary cause of neural networks’ vulnerability to adversarial
perturbation is their **linear nature**. **Moreover,this view yields a simple and fast method of generating adversarial examples.**

##### 观点：早期科学家们尝试解释对抗样本的产生主要聚焦于模型的高度非线性和过拟合，而Goodfellow则认为，神经网络在高维空间的线性行为才是导致对抗样本存在的真正原因。并且通过一堆量化的数据结果证明了它的观点，而且这个观点也得出了一个快速产生对抗样本的方法--Fast Gradient Sign Method （FGSM）



### **对抗样本的线性解释**：

![[公式]](https://www.zhihu.com/equation?tex=w%5E%7B%5Ctop+%7D%5Cwidetilde%7Bx%7D+%3D+w%5E%7B%5Ctop+%7Dx+%2B+w%5E%7B%5Ctop+%7D%5Ceta+%3D+w%5E%7B%5Ctop+%7Dx+%2B+w%5E%7B%5Ctop+%7Dsign%28w%29+) 

假设权重向量 ![[公式]](https://www.zhihu.com/equation?tex=w) 的每一个元素的平均值为 ![[公式]](https://www.zhihu.com/equation?tex=m) ，则对抗扰动对激活过程的影响为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvarepsilon+mn) , 因此，对抗扰动 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta) 对激活过程的影响会随着特征的维数 ![[公式]](https://www.zhihu.com/equation?tex=n) 线性地增长。对于高维问题，如果我们对模型输入每一维做出一点细微的改变，最终对模型输出结果的影响将是巨大的。 

### 非线性模型的线性扰动![img](https://pic3.zhimg.com/80/v2-cdea0e1fe15d390d4e0fd68ff106735e_720w.jpg) 

作者利用对抗样本的线性解释提出了一个快速产生对抗样本的方式，也即Fast Gradient Sign Method(FGSM)方法。假设模型的参数值为![\theta ](https://math.jianshu.com/math?formula=%5Ctheta%20)，模型的输入是 ![x](https://math.jianshu.com/math?formula=x%0A)，![y](https://math.jianshu.com/math?formula=y%0A)是模型对应的标签，![J(\theta ,x,y)](https://math.jianshu.com/math?formula=J(%5Ctheta%20%2Cx%2Cy))是损失函数，对某个特定的模型参数![\theta ](https://math.jianshu.com/math?formula=%5Ctheta%20)而言，**FGSM方法将损失函数近似线性化**，从而获得保证无穷范数限制的最优的扰动(即![\vert \vert \eta \vert \vert _{ \infty} <\epsilon ](https://math.jianshu.com/math?formula=%5Cvert%20%5Cvert%20%5Ceta%20%5Cvert%20%5Cvert%20_%7B%20%5Cinfty%7D%20%3C%5Cepsilon%20))

实验表明，FGSM这种简单的算法确实可以产生误分类的对抗样本，从而证明了作者假设的对抗样本的产生原因是由于模型的线性特性。同时，这种算法也可作为一种加速对抗训练的方法。 

**常规的分类模型训练在更新参数时都是将参数减去计算得到的梯度，这样就能使得损失值越来越小，从而模型预测对的概率越来越大。既然无目标攻击是希望模型将输入图像错分类成正确类别以外的其他任何一个类别都算攻击成功，那么只需要损失值越来越大就可以达到这个目标，也就是模型预测的概率中对应于真实标签的概率越小越好，这和原来的参数更新目的正好相反。因此我只需要在输入图像中加上计算得到的梯度方向，这样修改后的图像经过分类网络时的损失值就比修改前的图像经过分类网络时的损失值要大，换句话说，模型预测对的概率变小了。这就是FGSM算法的内容，一方面是基于输入图像计算梯度，另一方面更新输入图像时是加上梯度，而不是减去梯度，这和常见的分类模型更新参数正好背道而驰。
前面我们提到之所以采用梯度方向而不是采用梯度值是为了控制扰动的L∞距离，这只是其中的一部分。在Figure1中，梯度方向前一般会有一个权重参数e，这个权重参数可以用来控制攻击噪声的幅值，参数值越大，攻击强度也越大，肉眼也更容易观察到攻击噪声（因为输入图像归一化成0到1，所以图中e值只有0.07，换算成0到255的话差不多18个像素值），因此最终的攻击噪声就如下所示。因为FGSM算法的攻击噪声幅值评价指标是L∞，因此权重参数e和梯度方向就可以控制每个像素的最大变化值。** 

>  https://blog.csdn.net/u014380165/article/details/90723948 

### 对抗训练

Szegedy等人的研究表明，将干净数据和对抗样本混合起来一起训练在一定程度上可以使神经网络被正则化，而且，基于对抗样例的训练方式与其他的数据增强方法有所不同。

Goodfellow发现，基于快速梯度符号法对目标函数进行改进可以起到很好的正则化效果，改进后的损失函数形式为：

![[公式]](https://www.zhihu.com/equation?tex=%5Cwidetilde%7BJ%7D+%28%5Ctheta%2C+x%2C+y%29+%3D+%5Calpha+J%28%5Ctheta%2C+x%2C+y%29+%2B+%281-%5Calpha%29+J%28%5Ctheta%2C+x%2B%5Cepsilon+sign%28%5Cbigtriangledown_%7Bx%7DJ%28%5Ctheta+%2Cx%2Cy%29%29%2C+y+%29)

在试验中，作者设置 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha+%3D+0.5) （经验值）

### 总结

优点：这篇论文中，Goodfellow否定了Szegedy关于为什么神经网络易受到对抗样例攻击的解释，他认为**神经网络在高维空间中线性性质**才是导致对抗样本存在的真正原因。基于这种解释，Goodfellow提出了一种快速生成对抗样本的方法，即快速梯度符号法，这种方法的核心思想是沿着梯度的反方向添加扰动从而拉大对抗样本于原始样本的距离，因为Goodfellow认为在构造对抗样本时，我们更应该关心的是扰动的方向而不是扰动的数目。Goodfellow认为**对抗样本之所以有泛化性的原因是因为添加的扰动与模型的权重向量高度一致，而且不同的模型在被训练执行相同的任务时，从训练数据中学到的东西相似**。在这篇文章中，Goodfellow提出了对抗训练的思想，他认为对抗训练会导致训练过程中的正则化，而且其效果甚至超过了 ![dropout](https://math.jianshu.com/math?formula=dropout)。

不足：这篇文章中提出的快速梯度符号法存在明显的缺点，首先，这是一种**不定向的攻击**，只能让模型出错而无法做到定向攻击。而且这种攻击的**鲁棒性不强**，**添加的扰动容易在图片的预处理阶段被过滤掉**。尽管Googdfellow提出的对抗训练方式可以提高模型的泛化能力，从而在一定程度上防御对抗样本攻击，但这种防御方法只针对**一步对抗样本攻击**有效，攻击者仍可以针对新的网络构造其他的对抗样本。

>  https://zhuanlan.zhihu.com/p/33875223 
>
>  https://zhuanlan.zhihu.com/p/32784766 
>
>  https://zhuanlan.zhihu.com/p/94707659 
>
>  https://www.jianshu.com/p/f3ee515fa9f4 
>
>  https://blog.csdn.net/qq_24974989/article/details/88914086 
>
>  https://blog.csdn.net/u014380165/article/details/90723948 



# ADVERSARIAL EXAMPLES IN THE PHYSICAL WORLD (ICLR 2017)

Up to now, all previous work has assumed a threat model in which the adversary can feed data directly into the machine learning classifier. This is not always the case for systems operating in the physical world, for example those which are using signals from cameras and other sensors as input. This paper shows that even in such physical world scenarios, machine learning systems are vulnerable to adversarial examples.

以前的工作都是假定对手能直接对分类器上的输入数据进行操作。但是在现实世界中这种情况不总是满足的，比如用照相机拍摄的照片。这篇文章表明了即使在这样的现实世界场景中，机器学习系统对对抗样本也是脆弱的。

### 对抗样本的生成方法

基于单步/单次FGSM方法修改原图，提出了**Basic Iterative Method** 即FGSM的迭代版本，不同的是，**FGSM**每次是以大步方法e进行更新，而BIM是以多次小步进行更新，其中**0<a<e**，公式如下：

![image-20201008213143352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201008213143352.png)

鉴于FGSM和BIM都是无目标攻击，采用的是真实标签进行生成对抗样本，提出了**Least-Likely Class Iterative Method** 最小概率类别迭代方法，其中**0<a<e**

![image-20201008213446664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201008213446664.png)

### 实验一

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200505201332286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01UYW5kSEo=,size_16,color_FFFFFF,t_70) 

BIM提高epsilon值收益不高，继续提高其值效果不再明显。

LLC方法能在epsilon值很小的时候就达到比较好的效果。

FGSM随着epsilon值不断提高，性能不断提高，与迭代方法比较，它提升性能的原因是每次修改的epsilon值比较大，破坏了原始图像的大部分信息。而迭代方法每次修改的值介于0和epsilon之间。



### 实验二

 可以把这种操作看成一个变换T : X → T ( X ) , 如果真实世界中也存在对抗样本, 那么原本的adversarial samples 在经过这个变换之后很有可能也具有对抗的性质, 事实上, 实验显示的确, 虽然其对抗的程度有些许下降.

作者构建了一个指标(重构率)来衡量: ![img](https://upload-images.jianshu.io/upload_images/14709786-ce6ae2276a213841.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/1183/format/webp) 

 ![img](http://www.jjjccc.com/d/file/news/20200422/20190510144615477.png) 

 ![img](http://www.jjjccc.com/d/file/news/20200422/20190510150945635.png) 

从table中可以看出，观察Adv. images对应的那2列(Photos和Source images),使用fast方法产生的Adv. images在进行**Photo Transform**时，准确率变化不明显，而其他iter方法产生的结果则差异较大，可以发现，当进行Photo Transform后，其准确率反而提升了。这里作者给出的解释为：基于iter的方法进行的扰动比较细微，而Photo Transform把这些细微的扰动给抵消了。因此就出现了准确率反而提高的结果。

然后考虑**人为选择**的采样结果:(即在Clean image上正确分类，在Adversary image上错误分类的那些图片作为采样结果。 可以看出，结果与预期一致，原因同上。 

# 总结

优点：提出的攻击方法，一解决了噪声大小（afa介于0与epsilon之间），二是避免没有意义的错误分类（指定目标攻击），并且讨论了通过摄像头实际拍摄对对抗样本带来的影响。

不足：该物理攻击不是很符合实际，一是将对抗样本打印好正对着拍照，而实际生活中，物体应该是多角度，立体的，二是已知被攻击网络的具体攻击，为白盒攻击，但黑盒攻击才更符合实际。

 http://www.jjjccc.com/itjiaocheng/24881.html 

 https://blog.csdn.net/MTandHJ/article/details/105936065

 https://www.jianshu.com/p/2f3b15617236  