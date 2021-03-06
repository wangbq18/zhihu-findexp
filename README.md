# [2019年知乎看山杯专家发现算法大赛](https://www.biendata.com/competition/zhihu2019/)
队伍名：救救菜鸡吧   
A榜排名：第4   
B榜排名：第6  
最终分数：0.88696

首先感谢两名队友的实力带飞，我只是一个搬运工，大部分特征和方法都是队友想出来。

# 文件说明
- model。该文件夹下存放团队三人的已训练的lgb模型和相应的测试集特征文件，按照运行说明文档即可复现线上成绩。文件较大，请移步https://pan.baidu.com/s/1b10Gq9pcfHrsCeKURiXmHA
提取码：4ba2  下载。
- liuchen、shuxiufeng、sunrui。这三个文件夹分别存放团队三人的特征提取、模型训练的完整代码。请按照各自的说明文档进行运行。
- file。该文件夹存放一些文档信息，有知乎算法负责人的PPT分享、比赛解答论文、前排大佬的ppt分享(现场拍的照片)、海报等资料。

# 特征工程
这里主要写下效果较好的特征，全部的特征各位自行看代码吧 -^-
- 全局统计特征。主要构造如用户/问题的当天出现次数，用户/问题的当天当小时的出现次数，用户的历史正样本数等统计特征。   这类特征在结合B榜测试集统计时，线上提升巨大。通过数据分析，我们大致猜测出A榜是整个未来一周采样一半用户（后面前排大佬说是随机采样一半）发布的测试集，B榜就是另一半用户的测试样本。如果只在A榜或者B榜进行统计，训练集和测试集的特征分布很不一致，这也可能会导致线上线下不一致。在A榜的时候我们采取的解决办法是将训练集的用户进行采样，采样之后分成两部分单独统计特征。
- 时间差特征。主要构造用户/问题相邻两次邀请的时间差、用户距离上次回答的时间差、问题邀请距离问题创建的时间差。
- 转化率特征。主要构造了用户属性和话题的交叉转化率、用户属性和话题的交叉转化率。
- 用户历史点击序列特征。比如一个用户一共出现了5次，按时间排序的label假设为[1,0,1,0,0]，则用户第一次出现的特征为[],第二次出现的特征取值为[1],第三次出现的特征取值为[1,0],第四次出现的特征取值为[1,0,1],第五次出现的特征取值为[1,0,1,0]。可以直接将该特征作为字符串进行处理，也可以利用rnn进行处理。


# 线下验证方法
比赛过程中，线上线下分数一致性很重要，比赛中，多只团队反映线上线下差别很大，这里分享下我们团队尝试过的线下验证方式，最后一种方案基本做到了线上线下一致。

- 历史都可以作为特征，随机划分验证集。这种划分方式在提取特征的时候，该时刻之前的所有样本都用于提取特征，比如提取用户历史正样本数，就是统计该时刻之前的用户正样本数即可。这种划分方式有个问题就是每个样本的特征提取时间区间不一致，第1天的肯定比第二天的特征稀疏，这也导致误差较大。
- 固定特征提取区间，后三天为验证集。为了保证训练集、验证集、测试集特征的分布一致，我们使用固定长度的特征提取区间（invite表使用15天的，answer表使用30天的）。即训练集天数范围[3858, 3864]，训练集invite表使用的天数范围[3840, 3857]，answer表使用的天数范围[3827, 3857]；验证集天数范围[3865, 3867]，验证集invite表使用的天数范围[3846, 3863]，answer表使用的天数范围[3833, 3863]；验证集天数范围[3868, 3874]，验证集invite表使用的天数范围[3850, 3867]，answer表使用的天数范围[3837, 3867]。

# 其他大神的分享
- 第四名 lpl总冠军的分享（[github链接](https://github.com/VoldeMortzzz/2019Baai-zhihu-Cup-findexp-4th)）。
- 第七名 专家在哪里的分享 （[github链接](https://github.com/jt120/BAAI-zhihu-2019)）。


**整理不易，点个star支持下呗。**
