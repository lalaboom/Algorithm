第六章的漏电用户分析

说明：

相对简单的二分类问题，特征也都是数字，基本不用进行特征处理

感觉这一章的重点在于缺失值处理和不同模型的分类差异

在这里列一下主要学到的东西（可能有些知识认识的不是很全面）：

1.源代码中采用从random库中导入shuffle，我用的一篇博客中建议的从sklearn.utils中导入的shuffle函数，

   data在读取后是dataframe类型，需要用data = data.values变成ndarray类型

2.从keras 导入Activation 和Dense 可以自己构建神经网络，但是我想这只限于小规模神经网络

3.混淆矩阵

    1. 真阳性（True Positive，TP）：样本的真实类别是正例，并且模型预测的结果也是正例

    2. 真阴性（True Negative，TN）：样本的真实类别是负例，并且模型将其预测成为负例

    3. 假阳性（False Positive，FP）：样本的真实类别是负例，但是模型将其预测成为正例

    4. 假阴性（False Negative，FN）：样本的真实类别是正例，但是模型将其预测成为负例


4.roc曲线

    横坐标：1-Specificity，伪正类率(False positive rate， FPR)，预测为正但实际为负的样本占所有负例样本 的比例；

    纵坐标：Sensitivity，真正类率(True positive rate， TPR)，预测为正且实际为正的样本占所有正例样本 的比例
    
    (a) 理想情况下，TPR应该接近1，FPR应该接近0。ROC曲线上的每一个点对应于一个threshold，对于
    
     一个分类器，每个threshold下会有一个TPR和FPR。比如Threshold最大时，TP=FP=0，对应于原点；
     
     Threshold最小时，TN=FN=0，对应于右上角的点(1,1)。 
     
    (b) P和N得分不作为特征间距离d的一个函数，随着阈值theta增加，TP和FP都增加。

    横轴FPR：1-TNR，1-Specificity，FPR越大，预测正类中实际负类越多。
    
    纵轴TPR：Sensitivity(正类覆盖率)，TPR越大，预测正类中实际正类越多。
    
    理想目标：TPR=1，FPR=0，即图中(0,1)点，故ROC曲线越靠拢(0,1)点，
    
    越偏离45度对角线越好，Sensitivity、Specificity越大效果越好。
    
5.plt.annotate()函数用于标注文字。

    s 为注释文本内容

    xy 为被注释的坐标点

    xytext 为注释文字的坐标位置
    
  6. 拉格朗日插值：
  
    从我的经验看拉格朗日插值并不是一种常用的差值方法，其他插值如K值近邻，平均数，中位数反而应用较多
  
  7.Adam优化算法：
  
  Adam: A Method for Stochastic Optimization
  
  https://arxiv.org/abs/1412.6980v8
  
  https://blog.csdn.net/leadai/article/details/79178787
  
  https://blog.csdn.net/BVL10101111/article/details/72616516
  
  
