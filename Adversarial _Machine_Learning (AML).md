Adversarial Machine Learning (AML)的研究工作简单可以分为两个部分：**攻击**和**防御**。

攻击，即指如何生成对抗样本以使得机器学习模型产生错误的预测；防御，即指如何使机器学习模型对对抗样本更鲁棒。此外，近几年也出现了一些AML理论方向的工作。

攻击部分的工作从深度上来讲主要在于借助各种优化算法逐步提升生成对抗样本的攻击强度，从广度上来讲主要在于从一开始的针对CNN网络和图像数据扩展到更广的范围，如表格数据、时序数据，Auto-Encoder、强化学习等。

防御部分的主要工作是通过提出各种各样的正则项，来获得对某种或某些特定攻击鲁棒的模型。

近几年，关于AML相关的理论工作逐渐增多，在ICML2019和NIPS2019两个会议中就发表了数十篇相关工作，涉及的方面也非常广，包括解释性、收敛性、泛化性等，理论工作的百花齐放应该也是基础算法研究达到一定程度后必然出现的结果。

以下分别具体列举一些代表工作：

## **攻击：如何生成对抗样本**

攻击方式可以根据如下四种情况来分类：

1)黑盒/白盒攻击，主要区别在于是否需要知道模型信息；

2)targeted/non-targeted，主要区别在于生成对抗样本时，是否需要指定生成样本所属的类别;

3)Image specific / Universal, 主要区别在于是针对某张图片生成对抗样本，还是全局生成；

4)Perturbation norm：即生成对抗样本时所采用的范数。

首先介绍最主流的针对分类任务的攻击：

**针对分类任务的攻击：**

- **Box-constrained L-BFGS：**

**Intriguing properties of neural network**(CVPR 2014)：借助L-BFGS求解一个扰动，加到原始图片上生成对抗图片。作者单位：Google，New York University, Facebook.

- **Fast Gradient Sign Method (FGSM)：**

**Explaining and Harnessing Adversarial Examples** (ICLR 2015)：提出Fast Gradient Sign Method (FGSM)，以更高的效率求解生成对抗样本所需添加的扰动。作者单位：Google.

**Adversarial Machine Learning at Scale** (ICLR 2017): 将FGSM改进为non-targeted，即生成对抗样本时不需要指定其扰动后所属的类。作者单位：Google.

**Boosting Adversarial Attacks with Momentum** (CVPR 2018): 在FGSM的基础上增加momentum。作者单位：清华大学.

**Ensemble Adversarial Training: Attacks and Defenses** (ICLR 2018）：在FGSM的基础上进行改进，为训练样本添加迁移自其它模型的扰动信息。作者单位：Google.

- **Basic Iterative Method (BIM)：**

**Adversarial examples in the physical world** (CVPR 2016）：提出BIM方法，即优化时用若干小步代替一大步，进一步提出non-targeted版本，Iterative Least-likely Class Method (ILCM). 作者单位: Google.

- **Jacobian-based Saliency Map Attack (JSMA)：**

**The Limitations of Deep Learning in Adversarial Settings** (EURO S&P): 通过限制l_0 norm来得到对抗样本，目的是仅仅修改几个像素而不是整个图片。作者单位：Penn State University，University of Wisconsin-Madison，CMU.

- **One Pixel Attack：**

**One pixel attack for fooling deep neural networks** (IEEE Transactions on Evolutionary Computation 2019): 只修改一个像素生成对抗样本。作者单位：Kyushu University

- **Carlini and Wagner Attacks (C&W)：**

**Towards Evaluating the Robustness of Neural Networks**（IEEE S&P 2017）：通过限制0范数、2范数、无穷范数提出了新的攻击方式C&W。作者单位：Berkeley.

**ZOO: Zeroth Order Optimization based Black-box Attacks to Deep Neural Networks without Training Substitute Models** (CCS 2017)：在C&W的基础上，提出0阶优化的版本。作者单位：IBM Research.

**EAD: elastic-net attacks to deep neural networks via adversarial examples** (AAAI 2018): C&W的一个变种，C&W可以看作其一种特殊形式。作者单位：IBM T. J. Watson Research Center, The Cooper Union, New York, University of California, Tencent AI Lab

- **DeepFool：**

**DeepFool: a simple and accurate method to fool deep neural networks** (CVPR 2016): 给定一个图片，以迭代的方式寻找具有最小范数的对抗扰动。作者单位：Ecole Polytechnique F ´ ed´ erale de Lausanne

- **Universal Perturbation：**

**Universal adversarial perturbations** (CVPR 2017): 借助deepfool进一步提出universal perturbation，即扰动不是针对某一张图片，而是可以针对任意图片。作者单位：Universite de Lyon.

**Art of singular vectors and universal adversarial perturbations** (CVPR2018):利用神经网络的feature map来构建universal perturbation. 作者单位：Institute of Science and Technology，Moscow, Russia.

**Fast Feature Fool: A data-independent approach to universal adversarial perturbations** (BMVC2017)：generates the universal perturbations independent of data. 作者单位：Indian Institute of Science, Bangalore, India

- **Houdini：**

**Houdini: Fooling Deep Structured Prediction Models** （NIPS2017）：可以针对任务损失来定制攻击方式，不仅针对图片，在语音识别任务上也有效。作者单位：Facebook.

- **Projected Gradient Descent (PGD)：**

**Towards deep learning models resistant to adversarial attacks** (ICLR 2018):使用PGD算法来计算universal的扰动，是目前效果比较好的算法之一。 作者单位：MIT

- **Adversarial Transformation Networks (ATNs)：**

**Adversarial Transformation Networks: Learning to Generate Adversarial Examples** (AAAI 2018):通过神经网络来学习对抗样本的生成。作者单位: Google.

***Machine Learning as an Adversarial Service: Learning Black-Box Adversarial Examples** (ICML 2018): 将ATN改进到黑盒版本。作者单位：Massachusetts Institute of Technology

- **其它：**

**Black-box Adversarial Attacks with Limited Queries and Information** (ICML 2018):在queries有限的条件下进行黑盒攻击。作者单位：MIT

**Simple Black-box Adversarial Attacks** (ICML 2019)：提出了一种在queries有限的条件下进行黑盒攻击的方式，既可以是targeted又可以是non-targeted。作者单位: CMU, Uber.

**Improving Black-box Adversarial Attacks with a Transfer-based Prior** (NIPS 2019): 提出了一种基于先验指导的黑盒攻击手法，比以往的黑盒攻击强度更高。作者单位：清华大学。

**Imperceptible Adversarial Attacks on Tabular Data** (NIPS 2019 Workshop)：提出了表格数据上对抗样本的生成方式。作者单位：AXA Paris France

**Learning to Confuse: Generating Training Time Adversarial Data with Auto-Encoder** (NIPS 2019)：借助autoencoder生成时序的对抗样本。作者信息：南京大学，Ji Feng, Qi-Zhi Cai, Zhi-Hua Zhou.



除了分类任务还有少量工作关注其他任务上的对抗样本:

**分类任务之外的攻击**

- **Attacks on Autoencoders and Generative Models：**

**Adversarial images for variational autoencoders** (NIPS 2016, workshop on adversarial training): 提出针对autoencoder的攻击方式，使得重构的图片与原图差距大。作者单位：University of Campinas

**Adversarial examples for generative models** (ICLR 2017)：提出了针对生成式模型VAE和VAE-GAN的攻击方式。作者单位：National University of Singapore, Google, Berkeley.

- **Attack on Reinforcement Learning：**

**Tactics of Adversarial Attack on Deep Reinforcement Learning Agents** (IJCAI 2017)：提出针对深度强化学习的攻击方式。作者单位：National Tsing Hua University, NVIDIA.

## **防御：如何让机器学习模型更鲁棒**

防御的方法基本可以分为三类：

1. 修改训练过程或者数据输入**(Modified training/input)**
2. 修改模型结构**(Modified networks)**
3. 添加新的模型/组件**(Networks add-on)**

**Modified training / input：**

- **Brute-force adversarial training：**

**Improving the Robustness of Deep Neural Networks via Stability Training** (CVPR2016)：提出‘stability training’的方法，使神经网络对图片扰动更鲁棒。作者单位: Google

**Adversarial Training Methods for Semi-Supervised Text Classification** (ICLR 2017)：通过Virtual adversarial training使模型输出的分布更平滑。作者单位：Kyoto University

- **Data compression as defense：**

**A study of the effect of JPG compression on adversarial images** (CVPR 2016)：通过JPG压缩可以使图片对对抗扰动更为鲁棒。作者单位：University of Cambridge

还有其他一些类似针对不同的对抗方式和压缩方式的文章：

**Keeping the Bad Guys Out: Protecting and Vaccinating Deep Learning with JPEG Compression** (CVPR 2017) 作者单位：Georgia Institute of Technology，Intel.

**Countering Adversarial Images using Input Transformations**（ICLR 2018）作者单位：Cornell University.

**ME-Net: Towards Effective Adversarial Robustness with Matrix Estimation** (ICML 2019): 先随机丢掉一些像素，然后采用矩阵补全恢复。作者单位：MIT

- **Data randomization：**

**Adversarial Examples for Semantic Segmentation and Object Detection** (ICCV 2017): 发现random resizing, random padding可以降低对抗样本的影响。作者单位：Johns Hopkins University, Baidu.

**Modified networks：**

- **Deep Contractive Networks：**

**Towards Deep Neural Network Architectures Robust to Adversarial Examples** (CVPR 2015):借助contractive auto encoder的方法提升模型对对抗样本的鲁棒性。作者单位：Panasonic R&D Company of America

- **Regularization ：**

**A Unified Gradient Regularization Family for Adversarial Examples** (ICDM 2015)：引入梯度正则来提升对抗鲁棒性。作者单位：西安交通大学。

类似的做法还有很多，主要区别在于每篇文章提出了不同的正则项：

**Understanding Adversarial Training: Increasing Local Stability of Neural Nets through Robust Optimization** (Neurocomputing 2015)，作者单位: Yale University.

**Houdini: Fooling Deep Structured Prediction Models**（NIPS2017）：每层使用正则项来控制神经网络的Lipschitz常数来降低对抗样本的影响。作者单位：Facebook.

**DeepCloak: Masking Deep Neural Network Models for Robustness Against Adversarial Samples** (ICLR 2017)：通过加入一个masking layer来降低对抗样本的影响。作者单位：University of Virginia

**Improving the Adversarial Robustness and Interpretability of Deep Neural Networks by Regularizing their Input Gradients** (AAAI 2018)：提出了新的梯度正则项来提升鲁棒性。作者单位：Harvard University

**Improving Adversarial Robustness via Promoting Ensemble Diversity** （ICML 2019）：通过集成的方式来提升鲁棒性，提出了一个新的集成学习的正则项。作者单位：清华大学。

**Metric Learning for Adversarial Robustness**（NIPS 2019）：利用度量学习对表示空间增加一个正则项提升模型的鲁棒性。作者单位: Columbia University.

**Adversarial Robustness through Local Linearization** (NIPS 2019): 提出了一个新的梯度正则项提升模型鲁棒性。作者单位: Deepmind.

**Semidefinite relaxations for certifying robustness to adversarial examples** (NIPS 2019): 提出了一种semidefinite relaxation的方法，对任意Relu神经网络都能实现鲁棒性。作者单位：Stanford University.

**Deep Defense: Training DNNs with Improved Adversarial Robustness** （NIPS 2019） 在神经网络的目标函数中添加与对抗扰动有关的正则项以提升鲁棒性。作者单位：清华大学

**Towards Robust Detection of Adversarial Examples** (NIPS 2019)：通过最小化 reverse cross-entropy 来学到一个更容易区分对抗样本的表示。作者单位:清华大学。

- **Exploiting statistics：**

**Adversarial Examples Detection in Deep Networks with Convolutional Filter Statistics** (ICCV 2017): 使用卷积层的统计信息来判断一张图片是否是对抗样本，只能用于检测。作者单位： Oregon State University

**The Odds are Odd: A Statistical Test for Detecting Adversarial Example** (ICML 2019)：design a statistical test of a given sample’s log-odds robustness，只能用于检测。

**Network add-ons：**

**Defense against Universal Adversarial Perturbations** (CVPR 2018): 提出了一种针对universal perturbations生成的对抗样本的防御框架。作者单位：University of Western Australia

**Generative Adversarial Trainer: Defense to Adversarial Perturbations with GAN**. 借助GAN来提升模型鲁棒性，通过generator来生成扰动，然后让classifier正确区分干净的和扰动的图片。 作者单位: Seoul National University.

**APE-GAN: Adversarial Perturbation Elimination with GAN** (CVPR 2017)：使用生成模型来校正扰动的图片。作者单位：中科院计算所。

**MagNet: a Two-Pronged Defense against Adversarial Examples** (CCS 2017): 使用一个或多个detector来检测一张图片是不是对抗样本，只能用于检测。作者信息：上海科技大学

## **理论工作**

**Intriguing properties of neural network** (CVPR 2014)：指出DNN模型对图片中对抗样本的脆弱性，提出对抗样本的概念。作者单位：Google，New York University, Facebook.

**Explaining and Harnessing Adversarial Examples** (ICLR 2015)：进一步解释什么是对抗样本。作者单位：Google.

**Universal adversarial perturbations** (CVPR 2017): 指出DNN任意形式的对抗攻击都很脆弱，与图片无关。作者单位：Universite de Lyon.

**Analyzing the Robustness of Nearest Neighbors to Adversarial Examples** (ICML 2018): 分析KNN对对抗样本的鲁棒性，并提出一个新的1NN算法，具有鲁棒性保证。作者单位：University of California, San Diego

**Certified Adversarial Robustness via Randomized Smoothing** (ICML 2019)：分析l2_norm的鲁棒性和高斯噪声之间的关系，并提出了一种将任意分类器转换对l2_norm下的对抗扰动鲁棒的方法。作者单位：CMU

**Limitations of Adversarial Robustness: Strong No Free Lunch Theorem** (ICML 2019): 提出了对抗学习的no free lunch theorem，主要结论：if conditioned on a class label the data distribution satisfies the W2 Talagrand transportation-cost inequality any classifier can be adversarially fooled with high probability。作者单位：Criteo, Paris.

**On the Convergence and Robustness of Adversarial Training** （ICML 2019）：分析了对抗训练的收敛性，主要结论：to ensure better robustness, it is essential to use adversarial examples with better convergence quality at the later stages of training. 作者单位: 京东

**Interpreting Adversarially Trained Convolutional Neural Networks** (ICML 2019): 试图解释对抗训练的CNN如何识别物体，主要结论是adversarial training alleviates the texture bias of standard CNNs when trained on object recognition tasks, and helps CNNs learn a more shape-biased representation. 作者单位：北京大学

**Unlabeled Data Improves Adversarial Robustness** (NIPS 2019): 发现通过引入未标记数据和半监督学习(Self-Training)，可以提升模型的鲁棒性。作者单位：Stanford University.

**Theoretical evidence for adversarial robustness through randomization**（NIPS 2019）：从理论上分析randomization techniques可以提升模型鲁棒性。作者单位: PSL Research University， Facebook, Kyoto University, RIKEN.

**Are Labels Required for Improving Adversarial Robustness?** (NIPS 2019): 利用无标记数据来帮助提升模型鲁棒性，只用少量的标记数据就达到了与监督学习类似的效果。作者单位: Deepmind.