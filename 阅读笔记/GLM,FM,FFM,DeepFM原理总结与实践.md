ref：
> [FM系列算法解读（FM+FFM+DeepFM）](https://blog.csdn.net/hiwallace/article/details/81333604)
> [用Keras实现一个DeepFM](https://blog.csdn.net/songbinxu/article/details/80151814)
> [推荐系统遇上深度学习(三)--DeepFM模型理论和实践](https://mp.weixin.qq.com/s?__biz=MzI1MzY0MzE4Mg==&mid=2247483890&idx=1&sn=bd96178202507f9358b17f7c6aa91443&chksm=e9d01133dea7982568ae47e215c670bb86f144d2b47161af5b370471a5fd2a60a0d209aee328&scene=21#wechat_redirect)

## GLM：General Linear Model 
广义线性模型意为利用连接函数将各种分布（正态分布，二项分布，泊松分布）假设下的因变量Y与自变量X想联系起来，使用十分广泛。
原理：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826193129851.png)
线性分类：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826193412715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
## FM(Factorization Machine)
在某些场景下，比如文本信息处理中特征比较多，或者经过onehot处理之后，为了解决数据稀疏的情况下，特征怎样组合的问题，我们将不同特征进行线性组合，如图中公式一，或者线性组合后在加上两两组合<Vi，Vj>XiXj，如图中公式二，![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082619364184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
Vi是特征分量的辅助向量
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826194212548.png)
扩展到多维：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826194254510.png)
- SVM的二元特征交叉参数是独立的，而FM的二元特征交叉参数是两个k维的向量vi、vj，交叉参数就不是独立的，而是相互影响的。
- FM可以在原始形式下进行优化学习，而基于kernel的非线性SVM通常需要在对偶形式下进行
- FM的模型预测是与训练样本独立，而SVM则与部分训练样本有关，即支持向量
## FFM(Field-aware Factorization Machine)
one-hot类型的变量，会导致数据特征的稀疏。未解决这个问题，FFM在FM的基础上进一步改进，在模型中引入类别的概念，即field。将同一个field的特征单独进行one-hot，因此在FFM中，每一维特征都会针对其他特征的每个field，分别学习一个隐变量，该隐变量不仅与特征相关，也与field相关。 
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082619450337.png)
FFM主要用来评估站内的CTR和CVR，即一个用户对一个商品的潜在点击率和点击后的转化率。 
为了使用FFM方法，所有的特征必须转换成“field_id:feat_id:value”格式，field_id代表特征所属field的编号，feat_id是特征编号，value是特征的值。数值型的特征比较容易处理，只需分配单独的field编号，如用户评论得分、商品的历史CTR/CVR等。categorical特征需要经过One-Hot编码成数值型，编码产生的所有特征同属于一个field，而特征的值只能是0或1，如用户的性别、年龄段，商品的品类id等。除此之外，还有第三类特征，如用户浏览/购买品类，有多个品类id且用一个数值衡量用户浏览或购买每个品类商品的数量。这类特征按照categorical特征处理，不同的只是特征的值不是0或1，而是代表用户浏览或购买数量的数值。按前述方法得到field_id之后，再对转换后特征顺序编号，得到feat_id，特征的值也可以按照之前的方法获得。 

> - 样本归一化：FFM默认是进行样本数据的归一化，即 为真；若此参数设置为假，很容易造成数据inf溢出，进而引起梯度计算的nan错误。因此，样本层面的数据是推荐进行归一化的。
特征归一化：CTR/CVR模型采用了多种类型的源特征，包括数值型和categorical类型等。但是，categorical类编码后的特征取值只有0或1，较大的数值型特征会造成样本归一化后categorical类生成特征的值非常小，没有区分性。例如，一条用户-商品记录，用户为“男”性，商品的销量是5000个（假设其它特征的值为零），那么归一化后特征“sex=male”（性别为男）的值略小于0.0002，而“volume”（销量）的值近似为1。特征“sex=male”在这个样本中的作用几乎可以忽略不计，这是相当不合理的。因此，将源数值型特征的值归一化到 是非常必要的。
省略零值特征：从FFM模型的表达式可以看出，零值特征对模型完全没有贡献。包含零值特征的一次项和组合项均为零，对于训练模型参数或者目标值预估是没有作用的。因此，可以省去零值特征，提高FFM模型训练和预测的速度，这也是稀疏样本采用FFM的显著优势。

## DeepFM
FM通过对于每一位特征的隐变量内积来提取特征组合，最后的结果也不错，虽然理论上FM可以对高阶特征组合进行建模，但实际上因为计算复杂度原因，一般都只用到了二阶特征组合。对于高阶特征组合来说，我们很自然想到多层神经网络DNN。
### FM神经网络结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826194814334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
DeepFM结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826194847861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
Deep部分：
深度部分是一个前馈神经网络，与图像或语音类的输入不同，CTR的输入一般是极其稀疏的，因此需要重新设计网络结构。在第一层隐藏层之前，引入一个嵌入层来完成输入向量压缩到低位稠密向量
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082619493840.png)
有学者将DeepFM与当前流行的应用于CTR的神经网络做了对比，DeepFM的效果较好
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190826195022712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RlbmdjaGVuZ3R1NDEzOQ==,size_16,color_FFFFFF,t_70)
还未完。。。
