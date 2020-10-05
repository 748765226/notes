http://mp.ofweek.com/ai/a645673721126

人工智能之机器学习主要有三大类:1)分类;2)回归;3)聚类



回归不是单一的有监督学习技术，而是许多技术所属的整个类别。回归的目的是预测数值型的目标值，如预测商品价格、未来几天的PM2.5等。最直接的办法是依据输入写出一个目标值的计算公式，该公式就是所谓的回归方程（regressionequation）。求回归方程中的回归系数的过程就是回归。回归是对真实值的一种逼近预测。回归是统计学中最有力的算法之一。



回归概念：  回归是一个数学术语,指研究一组随机变量(Y1，Y2 ，…，Yi)和另一组(X1，X2，…，Xk)变量之间关系的统计分析方法，又称多重回归分析。其中, X1、X2，…，Xk是自变量,Y1，Y2，…，Yi是因变量。



**常见的回归种类有：线性回归、曲线回归、逻辑回归等。**

如果**拟合函数为参数未知的线性函数**，即因变量和自变量为线性关系时，则称为**线性回归**。

如果**拟合函数为参数未知的非线性函数**，则称为**非线性或曲线回归**。非线性函数的求解一般可分为将非线性变换成线性和不能变换成线性两大类。
 1） 变换成线性：处理非线性回归的基本方法。通过变量变换，将非线性回归化为线性回归，然后用线性回归方法处理。一般采用线性迭代法、分段回归法、迭代最小二乘法等。
 2）不能变换成线性：基于回归问题的最小二乘法，在求误差平方和最小的极值问题上，应用了最优化方法中对无约束极值问题的一种数学解法——单纯形法。该算法比较简单,收敛效果和收敛速度都比较理想。

将result归一化到[0, 1]区间，即使**用一个逻辑方程将线性回归归一化，称为逻辑回归(logisticregression)**。它是一种广义的线性回归。
 **逻辑回归(logistic regression)可分为二元逻辑回归、多元逻辑回归**。
 逻辑回归(logistic regression)是与线性回归相对应的一种分类方法。该算法的基本概念由线性回归推导而出。逻辑回归通过逻辑函数（即 Sigmoid 函数）将预测映射到 0 到 1 中间，因此预测值就可以看成某个类别的概率。



https://blog.csdn.net/weixin_39445556/article/details/81416133

**多元线性回归**就是:用多个x(变量或属性)与结果y的关系式 来描述一些散列点之间的共同特性.
 这些**x和一个y关系的图像并不完全满足任意两点之间的关系(两点一线),但这条直线是综合所有的点,最适合描述他们共同特性的,因为他到所有点的距离之和最小也就是总体误差最小.**

**最大似然估计**的意思就是最大可能性估计,其内容为:如果两件事A,B相互独立,那么A和B同时发生的概率满足公式                P(A , B) = P(A) * P(B)          P(x)表示事件x发生的概率.

回归的意思是**用一条直线来概括所有点的分布规律,并不是来描述所有点的函数**,因为不可能存在一条直线连接所有的散列点.所以我们计算出的值是有误差的,或者说**我们回归出的这条直线是有误差的**.我们回归出的这条线的目的是用来预测下一个点的位置.

**数学中并没有一种方法来直接求得什么情况下几个事件同时发生的概率最大.所以引用概率密度函数.**
**一个随机变量发生的概率符合高斯分布(也叫正太分布)，高斯分布的概率密度函数还是高斯分布**

 ![img](https://img-blog.csdn.net/20180806200221895?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTQ0NTU1Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

公式中x为实际值,u为预测值.在多元线性回归中,x就是实际的y,u就是θ_{T} * X.

 既然说我们要总结的事件是相互独立的,那么这里的每个事件肯定都是一个随机事件,也叫随机变量.所以我们要归纳的每个事件的发生概率都符合高斯分布.

什么是概率密度函数呢?它指的就是一个事件发生的概率有多大,当事件x带入上面公式得到的值越大,证明其发生的概率也越大.需要注意,得到的并不是事件x发生的概率,而只是知道公式的值同发生的概率呈正比而已.

 ![img](https://img-blog.csdn.net/20180806201332904?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTQ0NTU1Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

求得所有的时间发生概率最大就是求得所有的事件概率密度函数结果的乘积最大,则得到:

 ![img](https://img-blog.csdn.net/20180806201603858?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTQ0NTU1Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

  求得最大时W的值,则总结出了所有事件符合的规律.求解过程如下(这里记住,我们求得的是什么情况下函数的值最大,并不是求得函数的解):

 ![img](https://img-blog.csdn.net/20180806201922886?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTQ0NTU1Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

公式中,m为样本的个数,π和σ为常数,不影响表达式的大小.所以去掉所有的常数项得到公式:     

​         ![img](https://img-blog.csdn.net/20180806202139182?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTQ0NTU1Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)              

因为得到的公式是一个常数减去这个公式,所以求得概率密度函数的最大值就是求得这个公式的最小值.这个公式是一个数的平方,在我国数学资料中把他叫做最小二乘公式.所以多元线性回归的本质就是最小二乘.



**梯度和梯度下降**

https://blog.csdn.net/weixin_39445556/article/details/83661219

**L-BFGS**

https://blog.csdn.net/weixin_39445556/article/details/84502260

**避免过拟合的方法：** 降低参数空间的维度或者降低每个维度上的有效规模（effective size）。降低参数数量的方法包括greedy constructive learning、剪枝和权重共享等。降低每个参数维度的有效规模的方法主要是正则化，如权重衰变（weight decay）和早停法（early stopping）等。

 https://www.datalearner.com/blog/1051537860479157 

 https://blog.csdn.net/lqz790192593/article/details/83045114 

 https://blog.csdn.net/qq_28869927/article/details/85249524 

##### 常用的向量与矩阵的范数总结[L0、L1、L2范数]

 https://blog.csdn.net/baidu_36415362/article/details/107224116 