# 5.3 反向传播

## Hebbian 学习法则和 Delta 学习法则

###  赫布理论和赫布学习法则

1. 赫布理论：1949年，由唐纳德·赫布提出，又被称为赫布定律（Hebb's rule）、赫布假说（Hebb's postulate）、细胞结集理论（cell assembly theory）等。他如此表述这一理论：

   > 我们可以假定，反射活动的持续与重复会导致神经元稳定性的持久性提升……当神经元A的轴突与神经元B很近并参与了对B的重复持续的兴奋时，这两个神经元或其中一个便会发生某些生长过程或代谢变化，致使A作为能使B兴奋的细胞之一，它的效能增强了。

2. 赫布理论描述了突触可塑性的基本原理，即突触前神经元向突触后神经元的持续重复的刺激，可以导致突触传递效能的增加。基于赫布理论的学习方法被称为赫布型学习。

3. 赫布型学习（赫布学习法则）：是一个无监督学习规则，这种学习的结果是使网络能够提取训练集的统计特性，从而把输入信息按照它们的相似性程度划分为若干类。 这一点与人类观察和认识世界的过程非常吻合，人类观察和认识世界在相当程度上就是在根据事物的统计特征进行分类。 Hebb学习规则只根据神经元连接间的激活水平改变权值，因此这种方法又称为相关学习或并联学习。

4. 公式如下，其中$\Delta w$为连接强度（权重）的变化量，$y_i,y_j$分别表示突触前、后神经元的兴奋程度（输出）。
5. 
$$
   \Delta w=\gamma y_iy_j
$$
   

### Delta学习法则

1. Delta学习法则（Widrow-Hoff Rule, LMS Rule）是于1960年由Widrow和Hoff提出的一种关于神经网络的学习定律，内容如下：

   > 如果输出于正确答案之间的差值越大，则需要设置的权重的修正量也越大；
   >
   > 如果输入越大，则需要设置的权重的修正量也越大。

2. 公式如下：
   
$$
   \Delta w=\eta(y_j-t)y_i
$$

   其中，$t$为正确答案的值，$\eta$为一个被称作“学习系数”的常数。

### Back Propagation 算法

- 由上面可以得知：神经网络的学习主要蕴含在权重和阈值中，多层网络使用上面简单感知机的权重调整规则显然不够用了，BP神经网络算法即误差逆传播算法（Error Back Propagation）正是为学习多层前馈神经网络而设计，BP神经网络算法是迄今为止最成功的的神经网络学习算法。

- 一般而言，只需包含一个足够多神经元的隐层，就能以任意精度逼近任意复杂度的连续函数[Hornik et al.,1989]，故下面以训练单隐层的前馈神经网络为例，介绍BP神经网络的算法思想。

![8.png](https://i.loli.net/2018/10/17/5bc72cbb92ff5.png)

   1. 上图为一个单隐层前馈神经网络的拓扑结构，BP神经网络算法也使用梯度下降法（gradient descent），以单个样本的均方误差的负梯度方向对权重进行调节。

   2. 可以看出：BP算法首先将误差反向传播给隐层神经元，调节隐层到输出层的连接权重与输出层神经元的阈值；接着根据隐含层神经元的均方误差，来调节输入层到隐含层的连接权值与隐含层神经元的阈值。BP算法基本的推导过程与感知机的推导过程原理是相同的，下面给出调整隐含层到输出层的权重调整规则的推导过程：
   
$$
   E_{k}=\frac{1}{2} \sum_{j=1}^{1}\left(\hat{y_{j}}-y_{j}\right)^{2}
$$

其中，$\hat{y}_{j}=f\left(\sum_{h=1}^{q} w_{h j} b_{h}-\theta_{j}\right)$。故$E_k$是关于$W_{hj}$与$\theta_{j}$的多项式。令$b_{q+1}=-1$​，有

$$
   \hat{y_{j}}=f\left(\sum_{n=1}^{q+1} W_{hj}b_{n}\right)
$$

根据链式法则，有

$$
   \begin{aligned}\frac{\partial E_{k}}{\partial w_{h}} & =\frac{\partial E_{k}}{\partial \hat{y}_{j}} \cdot \frac{\partial \hat{y}_{j}}{\partial \beta_{j}} \cdot \frac{\partial \beta_{j}}{\partial w_{n j}} \\& =\left(\hat{y_{j}}-y_{j}\right) \cdot f^{\prime}\left(\beta_{j}\right) \cdot b_{h} \\& =\left(\hat{y_{j}}-y_{j}\right) \cdot \frac{1}{1+e^{-\beta_{j}}} \cdot\left(1-\frac{1}{1+e^{-\beta_{j}}}\right) \cdot b_{h} \\& =\left(\hat{y_{j}}-y_{j}\right) \cdot \hat{y_{j}} \cdot\left(1-\hat{y}_{j}\right) \cdot b_{h}\end{aligned}
$$

故$\Delta w_{r j}=-\dfrac{\partial E_{k}}{\partial w_{n j}} \cdot \eta, \eta \in(0,1)$
所以，有

$$
   \Delta w_{n j}=\eta\left(y_{j}-\hat{y_{j}}\right) \cdot \hat{y_{j}} \cdot\left(1-\hat{y_{j}}\right) \cdot b_{h}
$$


$$
   \Delta \theta_{j}=-\eta\left(y_{j}-\hat{y_{j}}\right) y_{j}\left(1-\hat{y_{j}}\right),(\text{i.e. }b_n=-1)
$$


$$
   \Delta v_{i n}=\eta b_{n}\left(1-b_{n}\right) \sum_{j=1}^{l} w_{n j} g_{j} x_{i}
$$


$$
   \Delta r_{n}=-\eta b_{n}\left(1-b_{n}\right) \sum_{j=1}^{l} w_{n j} g_{i}
$$

梯度项：$g_{i}=\hat{y_{j}}\left(1-\hat{y_{j}}\right)\left(y_{j}-\hat{y_{j}}\right)$。

- 学习率η∈（0，1）控制着沿反梯度方向下降的步长，若步长太大则下降太快容易产生震荡，若步长太小则收敛速度太慢，一般地常把η设置为0.1，有时更新权重时会将输出层与隐含层设置为不同的学习率。BP算法的基本流程如下所示：

![10.png](https://i.loli.net/2018/10/17/5bc72cbb59e99.png)

- BP算法的更新规则是基于每个样本的预测值与真实类标的均方误差来进行权值调节，即BP算法每次更新只针对于单个样例。需要注意的是：BP算法的最终目标是要最小化整个训练集D上的累积误差，即：

$$
   E=\frac{1}{m}\sum_{k=1}^m E_k
$$

如果基于累积误差最小化的更新规则，则得到了累积误差逆传播算法（accumulated error backpropagation），即每次读取全部的数据集一遍，进行一轮学习，从而基于当前的累积误差进行权值调整，因此参数更新的频率相比标准BP算法低了很多，但在很多任务中，尤其是在数据量很大的时候，往往标准BP算法会获得较好的结果。另外对于如何设置隐层神经元个数的问题，至今仍然没有好的解决方案，常使用“试错法”进行调整。

前面提到，BP神经网络强大的学习能力常常容易造成过拟合问题，有以下两种策略来缓解BP网络的过拟合问题：

- 早停：将数据分为训练集与测试集，训练集用于学习，测试集用于评估性能，若在训练过程中，训练集的累积误差降低，而测试集的累积误差升高，则停止训练。
- 引入正则化（regularization）：基本思想是在累积误差函数中增加一个用于描述网络复杂度的部分，例如所有权值与阈值的平方和，其中λ∈（0,1）用于对累积经验误差与网络复杂度这两项进行折中，常通过交叉验证法来估计。

$$
   E=\lambda\frac{1}{m}\sum_{k=1}^m E_k+(1-\lambda)\sum_iw_i^2
$$

