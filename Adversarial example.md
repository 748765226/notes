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

### 总结

优点：提出的攻击方法，一解决了噪声大小（afa介于0与epsilon之间），二是避免没有意义的错误分类，并且讨论了通过摄像头实际拍摄对对抗样本带来的影响。

不足：该物理攻击不是很符合实际，一是将对抗样本打印好正对着拍照，而实际生活中，物体应该是多角度，立体的，二是已知被攻击网络的具体攻击，为白盒攻击，但黑盒攻击才更符合实际。

 http://www.jjjccc.com/itjiaocheng/24881.html 

 https://blog.csdn.net/MTandHJ/article/details/105936065

 https://www.jianshu.com/p/2f3b15617236  



# The Limitations of Deep Learning in Adversarial Settings

# （Papernot EuroS&P 2016）

### 文章概述

与之前的基于**提高原始类别标记的损失函数或者降低目标类别标记的损失函数**的方式不同，这篇文章提出直接增加神经网络对目标类别的预测值。换句话说，之前的对抗样本的扰动方向都是损失函数的梯度方向（无论是原始类别标记的损失函数还是目标类别标记的损失函数），该论文生成的对抗样本的扰动方向是目标类别标记的预测值的梯度方向，作者将这个梯度称为**前向梯度（forward derivative）**。本文主要介绍了一种新的对抗样本攻击方法，利用输入特征到输出值之间的**对抗性显著性**，达到只需修改少量的输入值即可误分类的目的。该攻击方法属于**targeted攻击**。文章指出**Forward derivatives approaches are much more powerful than gradient descent techniques used in prior systems.**

### 实施细节

 ![img](https://img-blog.csdnimg.cn/2019062014502317.png) 

#### Step 1: 计算前向导数

 ![img](https://img-blog.csdnimg.cn/20190620150409495.png) 

#### Step 2: 基于前向导数构造显著图S 

 ![img](https://img-blog.csdnimg.cn/20190620153442764.png) 

 ![img](https://img-blog.csdnimg.cn/20190620153601147.png) 

#### Step 3: 利用θ 修改输入特征imax 

但是在实际操作时，每次选择一个像素点太严格(很难满足otherwise的条件，(上面的方式1和方式2))，于是作者提出了采用一组像素点而不是一个像素点的方法。

####  ![img](https://img-blog.csdnimg.cn/20190620180350967.png)   

 ![img](https://img-blog.csdnimg.cn/20190620180250952.png) 

上图中的算法2就是作者实际采用的方法，算法3是使用像素点对的启发式搜索算法。其中，作者在算法3想要找到的( p 1 , p 2 ) 满足下式

 ![img](https://img-blog.csdnimg.cn/20190620180745567.png) 

**像素点对的组合方式过多，计算存在负担**

**实际操作：作者采用的是最后1层的隐藏层输出结果来进行前向传播的计算而非最后的output probability layer的结果。原因是这两个层之间的逻辑回归计算引入的极端变化以确保概率之和为1，从而导致极端导数值。这降低了神经元如何被不同输入激活的信息质量，并导致前向导数在生成显著性映射时精度较低。此外最后一个隐层也由10个神经元组成，每个神经元对应一个数字类0到9，处理起来效果更好。**

 ![img](https://img-blog.csdnimg.cn/20190620182849255.png)  ![img](https://img-blog.csdnimg.cn/20190620182815242.png) 

对于前面的2种显著图的生成方式，作者也展示了相应的结果：下面左图是方式1生成的结果，右图是方式2生成的结果。可以看出，方式2产生的结果比较不容易被人类察觉。但是方式2在扰动阈值的限制下成功攻击的概率只有64.7% 而方式1则能达到97%(Fig. 11)。这也是合理的，因为去除像素降低了信息熵，从而使得DNNs更难提取分类样本所需的信息。

### 参考

https://blog.csdn.net/Invokar/article/details/96697862

https://blog.csdn.net/qq_44109982/article/details/107682952

https://blog.csdn.net/qq_36415775/article/details/89205794

https://zhuanlan.zhihu.com/p/33501618

https://mathpretty.com/10683.html **(Sliency Map)**

https://blog.csdn.net/zxyhhjs2017/article/details/88640547



# One Pixel Attack for Fooling Deep Neural Networks(CVPR2017)

# Nicolas Papernot 尼古拉斯 Papernot

### Motivation

most of the previous attacks did not consider extremely limited scenarios for adversarial attacks, namely the modification might be excessive (i.e., the amount of modified pixels is fairly large) such that it may be perceptible to human eyes.

### One Pixel Attack![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704160653172.png) 

如图所示，这个4×4的表格代表着加在origin image上的perturbations，一般的attack是相当于改变整张图片的像素值来产生对抗样本，而该方法仅改变one pixel即可实现对抗攻击。如果改变的这一个pixel整好与背景颜色相差不大，那图像会非常棒。
 通过损失函数来看，两者区别就是约束部分： 

一般的attack：       

​									 				     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704183051301.png)              

是让产生的perturbations小于L或者L-infinity normal必须要小于某个值  而one pixel attack：               

​     												  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704151340171.png)        

是让产生的perturbations的L0范数小于d（d=1），即perturbations种只有一个像素点不为0，也就是对抗样本实际上只改变了一个像素点。

现在问题来了，How do we find the exact pixel and value？有一种笨拙的办法，将224*224个像素点一一更改，找到最优的那个值。但是时间会很长，因为你每次改变你的像素值，都要放到model里去检测一下。下面作者很聪明，将差分进化算法应用了进来。

### Differential Evolution  

差分进化算法一共分为4部分： 

**初始化种群：**

​    												     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704152901701.png)       

 **变异：**     

​														  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704152957693.png)         

 **交叉：**     

​         												 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704153057955.png) 

**挑选：**            

​														   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200704153136525.png)  

初始化种群的时候，我们种群中的每个个体都是（x,y,R,G,B)这样一个5维的向量，变异-挑选三个过程反复进行。初始种群内产生变异，变异种群与原种群进行交叉，再对交叉后产生的种群与原种群进行对比。会使种群不断的淘汰差的个体，留下优良个体，由优良个体组成的新一代种群继续进行此操作。大约进行6次，即可得到较好的种群，可从中挑选最优的值作为最终的结果。使用DE 有什么好处： 

1.有更高的几率可以找到全局最优点。因为变异产生的种群具有多样性的特点且至少使用了一组候选方案。
2.需要目标模型很少的信息，相比于FGSM来说，DE不需要梯度信息，因此不需要model太多细节。



https://blog.csdn.net/qq_48996375/article/details/107128978

https://www.cnblogs.com/fourmi/p/10117832.html

**差分进化算法** 

https://blog.csdn.net/qq_37423198/article/details/77856744 

https://blog.csdn.net/changyuanchn/article/details/80331134



# Towards Evaluating the Robustness of Neural Networks（C&W）S&P(2017)

**补充知识：蒸馏防御深度神经网络中的对抗扰动**

**《Distillation as a Defense to Adversarial Perturbations against Deep Neural Networks》** 

**参考：**https://zhuanlan.zhihu.com/p/31177892

**范数：**https://blog.csdn.net/zouxy09/article/details/24971995/
$$
对于距离度量而言，Lp 距离常常被写作||x−x′||p，其 p 范数被定义为：||v||_p=(\sum_{i=1}^n|v_i|^p)^{\frac{1}{p}}
$$
![image-20201021160001348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201021160001348.png)

**简介：**

证明defensive distillation不能显著地提高模型的鲁棒性 

介绍了3种新的攻击算法，可以在distilled和undistilled神经网络达到100%的攻击成功率 

本文的攻击相比于以前的攻击通常会更有效 

本文的对抗性样本可以从不安全的网络模型迁移到distilled(安全)的网络模型去 

数据集：MNIST、CIFAR-10、ImageNet 

本文主要研究Targeted Attacks 由于defensive distillation并不能真正地避免对抗样本的出现，作者认为原因可能是对抗样本的存在原因在于神经网络局部线性的性质(这是一种说法，也有其他说法)

This paper makes the following contributions:   

1.We introduce three new attacks for the L0, L2, and L_infinite distance metrics. Our attacks are significantly more effective than previous approaches. Our L0 attack is the first published attack that can cause targeted misclassification on the ImageNet dataset.
 2.We apply these attacks to defensive distillation and discover that distillation provides little security benefit over un-distilled networks.
 3.We propose using high-confidence adversarial examples in a simple transferability test to evaluate defenses, and show this test breaks defensive distillation.
 4.We systematically evaluate the choice of the objective function for finding adversarial examples, and show that the choice can dramatically impact the efficacy of an attack.



### **Approach**

作者把**构建对抗样本**的过程转化为一个**最优化问题**（包括一个等式约束和一个box约束）：

 ![img](https://img-blog.csdnimg.cn/20190717163317543.png) 

但是其中的等式约束难以求导，所以作者将这一等式进行了转化：
$$
C(x+δ)=t⇒f(x+δ)≤0
$$
并给出了7种**Object Function：**

 ![img](https://img-blog.csdnimg.cn/20190717165048347.png) 

 最后转化为：

![img](https://img-blog.csdnimg.cn/20190717165201956.png)

其中 c>0 是一个惩罚因子，用于权衡目标和约束的重要性，有点像正则化，论文通过**二分查找法**来选择合适的 c，下图是 c 的敏感度，可以看出在 L2 约束和 f6 目标函数的前提下，当 c<1，攻击成功率很低。在 c>1 之后，攻击效果提升不大，但成功率很高。

最初将 c 设为一个非常低的值 (例如，10−4)。然后用这个 c 值来产生对抗样本。如果失败了，则将 c 加倍，然后再试一次，直到成功为止。如果 c 超过一个固定的阈值 (例如，1010)，则中止搜索。



​											 ![img](https://lifengjun.xin/2020/03/14/%E3%80%90%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%E3%80%91-Towards-Evaluating-the-Robustness-of-Neural-Networks/4.png) 

### Box constraints

为了确保生成有效的图片，对于像素点的扰动 δ 存在着约束
$$
0≤xi+δi≤1
$$
在优化问题中，这个被称为 “**盒约束**”。该论文研究了三种不同方法来解决这个问题： 

 第一个方法是 **Projected gradient descent**，在每次迭代中，执行一个单步标准梯度下降后，将所有坐标约束到 [0,1] 的范围内。简单粗暴地对其裁剪, 大于1的为1, 小于0的为0, 这个方法的性能不佳，原因在于它将剪辑后的图片作为下一次迭代的输入。

 第二个方法是 **Clipped gradient descent**，上述方法存在一个问题就是算法可能会卡在一个平坦的点上，即当像素增加了一个比允许的最大值大得多的分量的时候，再将其约束到 1，那么偏导数就会变成 0。所以这个方法将盒约束整合到目标函数中，即目标函数从 f(x+δ) 转化为
$$
f(min(max(x+δ,0),1))
$$

 第三个方式是 **Change of variables**，该方法引入一个新的变量 ω，将上述优化 δ 的问题转化为优化 ω，通过定义：

 													![img](https://img-blog.csdnimg.cn/20190717173558575.png) 

因为−1≤tanh(ωi)≤1，所以0≤xi+δi≤1 是成立的，这样的转化允许我们使用其他不支持盒约束的算法进行优化，论文尝试了三种求解方法：标准梯度下降法、动量梯度下降法以及 Adam 算法，并且发现这三种算法都得到相同质量的解，然而 Adam 算法的收敛速度最快。



最后的结果如下:  

**Best Case**: target label是最容易攻击的label 

**Worst Case**：target label是最难攻击的label 

**Average Case**：target label是随机挑选的label 

可以发现**Projected Descent**在处理box约束是优于其他两种策略，但**Change of Variable**寻求的扰动一般都是较小的，此外f 2 , f 3 , f 4效果欠佳，但f 6 在这7种策略中基本上是最好的：需要的扰动一般最小，且成功率高。

 ![img](https://img-blog.csdnimg.cn/20190717190508457.png) 

### Discretization 

对于有的图片，其像素数值是整数，则需要对攻击后的图像数据取整，即进行离散化，论文用的是**贪心网格搜索**。

### Three Attacks 

最后论文对L2、L0、L∞三种约束提出了三种攻击方法。（后两种攻击都是基于L2 Attack）

**L2 Attack**

下面的f选择的就是之前测试结果中效果最好的f6, 只是对k解除了必须为0的约束
$$
\text{minimize}\;||\frac{1}{2}(tanh(\omega)+1)-x||^2_2+c\,·\,f(\frac{1}{2}(tanh(\omega)+1)\\ f(x')=\text{max}(\max{\{Z(x')_i:i\ne t\}-Z(x')_t,-κ})
$$
参数k鼓励算法在寻找对抗样本的时候，将其归为target类时有更高的置信度，同时能够增加迁移率。原因就是他更加鼓励分为其他类时，最大的置信度要比target类的置信度会往k的差距优化。

![img](https://img-blog.csdnimg.cn/20190718095144152.png) 

**多起点梯度下降**  

梯度下降算法的主要问题是**贪心搜索不能保证找到最优解，陷入局部极小值**。为了弥补这一点，论文选择多个随机起点接近原始图像和运行梯度下降从每个点为固定次数的迭代。论文从半径为 r 的球上均匀地随机采样点，其中 r 是迄今为止发现的最接近的反例。从多个起点开始降低了梯度下降陷入一个糟糕的局部最小值的可能性。同时这也意味着 L2 attack 可以并行化，从而可以提高攻击的速度。

**L0 Attack**

因为L0距离是无法微分的，所以作者采用迭代的方法，使用L2 Attack来确定哪些像素不重要，对不重要的像素点就固定着，不断迭代，不断增加这个无用像素点集合，直至无点可优化。

简单说，就是通过L2 Attack来产生一个有效的对抗样本，然后通过 ![[公式]](https://www.zhihu.com/equation?tex=%5Cdelta%2Ag) 来表征像素点对产生对抗样本的贡献大小，g是对应梯度，直觉上，变化越小，梯度改变越大的点对产生对抗样本越具有重要作用。每次把影响最小的点移出可变像素点集合（也就是以后不能再变它了）
$$
i = \arg\min_i g_i \cdot \delta_i
$$
重复以上过程，直到L2 Attack再也找不到对抗样本。 

 **L∞ attack** 

对于无穷范数，假设我们使用类似于之前的： 
$$
\begin{equation}\nonumber \min c \cdot f(x+\delta) + ||\delta||_{\infty} \end{equation}
$$
无穷阶范数函数不是全可微分的，由于无穷阶范数由最大量决定，所以对应的梯度下降只会影响最大改变量的像素点，而对其他像素点没有任何影响，这样很低效！

并且这种情况下，很容易陷入我们不想要的一个次优解，在论文中给出了一种情况。

​									 ![img](https://pic2.zhimg.com/80/v2-d6c73a2d0cb274fc073e5c5a7f9aad31_720w.jpg) 

简言之，就是如果有两个势均力敌的点，然后梯度下降最后就是在这俩点之间来回晃，没啥进步。为了解决这个情况，作者提出了新的方法，思想是，不再惩罚最大点了，而是惩罚所有大点（设置一个阈值，表征大点）

​                                    ![img](https://pic2.zhimg.com/80/v2-a15a747a3eb6fd5cce1a2903a0608039_720w.jpg) 

在每次迭代之后，如果对所有的 i都有δi<τ，我们将 τ减少0.9倍并重复; 否则，我们终止搜索。再次，我们必须选择一个好的常数c用于L∞攻击。 我们采用与L0攻击相同的方法：首先将c设置为非常低的值，然后以此c值运行L∞攻击。 如果失败，我们加倍c并重试，直到成功。 如果c超过固定阈值，我们中止搜索。在每次迭代中使用“热启动”进行梯度下降，作者指出该算法的速度与之前的L2算法（使用单个起点）一样快。实际上0-范数攻击很慢，并且也不是很好的攻击方式，作者也并不推荐。 

### 参考

https://lifengjun.xin/2020/03/14/

https://blog.csdn.net/Invokar/article/details/96698653

https://blog.csdn.net/kearney1995/article/details/79904095

https://www.cnblogs.com/MTandHJ/p/12661003.html

https://zhuanlan.zhihu.com/p/77259550?from_voters_page=true