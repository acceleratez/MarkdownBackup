# 排序总览

## 分类

| 数据规模、存储特点 | 内部排序（内存即可容纳） | 外部排序（借助外部存储） |
| :----------------: | :----------------------: | :----------------------: |
|      输入形式      |         离线算法         |         在线算法         |
|  所依赖的体系结构  |           串行           |           并行           |
|  是否采用随机策略  |          确定式          |          随机式          |

## 比较树模型和复杂度下界

1. 比较树是一种二叉树，用于表示基于比较的算法，如排序和搜索算法。在比较树中，每个内部节点表示一个比较操作，每个叶节点表示一个可能的输出。
2. 比较树模型可以用来理解和分析基于比较的算法。例如，比较树的深度（最长路径的长度）表示了算法在最坏情况下需要进行的比较次数。这可以用来证明基于比较的排序算法的下界，即无论我们如何改进算法，任何基于比较的排序算法在最坏情况下都必须进行至少n log n次比较，其中n是要排序的元素的数量。
3. 复杂度下界：一般地，任一问题在最坏情况下的最低计算成本，即为该问题的**复杂度下界**。
4. 比较式算法：比较式算法（Comparison-Based Algorithm, CBA）是一类使用比较操作来解决问题的算法。在这类算法中，数据项之间的相对顺序是通过一对一的比较来确定的。排序算法是比较式算法的一个常见例子。

## 两条定理

1. Any algorithm that sorts by exchanging adjacent elements requires $\Omega(n^2)$ average time.

   > In order for a sorting algorithm to run in sub-quadratic time, it must do comparisons and exchanges between elements that are far apart.

2. No sorting algorithm based on key comparisons can possibly be faster than $\Omega(n\log n)$; (Proof Below)

## 估计下界

1. 考查任一CBA式算法A，设`CT(A)`为与之对应的一棵比较树。

2. 根据比较树的性质，算法A每一次运行所需的时间，将取决于其对应叶节点到根节点的距离 （称作叶节点的深度）；而算法A在最坏情况下的运行时间，将取决于比较树中所有叶节点的最大深度（称作该树的高度，记作h(CT(A))）。因此就渐进的意义而言，算法A的时间复杂度应不低于$\Theta\left(h\left(CT(A)\right)\right)$。

3. 则对于任一的CBA算法，其复杂度下界就应为比较树的最小高度。为了计算最小高度，只要考察树中的**叶**节点的数目。对于树高为`h`二叉树（在比较中，无非就是大（小）于或不大（小）于这两个结果，所以这里应为二叉树），其叶子节点的个数不可能多于$2^h$。反过来，有：

   > 若某一问题的输出结果不少于$N$种，则比较树中叶节点也不可能少于$N$个，树高$h$不可能低于$\log_2N$。

4. 对于CBA式的排序算法，就n个元素的排序问题而言，可能的输出共有$n!$种。此时，元素间不止可以判等，而且可以比较大小，故此时，比较树属于三叉树。仿照以上的内容，我们有：

   > 若某一规模为$n$的排序问题的输出结果不少于$N=n!$种，则三叉比较树种的叶子节点也不会少于$N$个，树高$h$不可能低于$\log_3N$。

   这里，使用Stirling逼近公式，有：

   

   > $$
   >  h\ge\left\lceil{\log_3(n!)}\right\rceil=\left\lceil{\log_3\text e\cdot\ln(n!)}\right\rceil=\Omega(n\log n).
   > $$
   >
   > 
   >
   > 由此可见，最坏情况下CBA式排序算法至少需要$\Omega(n\log n)$时间，其中n为待排序元素数目。

5. $\Omega(n\log n)$的下界是针对比较树模型而言的。事实上，还有很多不属此类的排序算法（桶排序、基数排序算法），而且其中一些算法在最坏情况下的运行时间，有可能低于这一下界，但与上述结论并不矛盾。
