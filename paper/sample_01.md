
# RADICAL ANALYSIS NETWORK FOR ZERO-SHOT LEARNING IN PRINTED CHINE CHARACTER RECOGNITION

Author: Jianshu Zhang, Yixing Zhu, Jun Du and Lirong Dai

Kedword: Chinese character, radical analysis, zero-shot learning, encoder-decoder, attention

## 核心问题提出

在中文字符的识别中，目前普遍的思路是对单个字符独立进行识别。但是中文有超过20000个字符，即使常用汉字也有3000多个，也就是说，如果我们使用常规的识别的方法的话，我们需要对20,000或3000多个类别进行分类。

## 解决问题的思路和方法

作者提出了一种的巧妙的方法岁汉字进行识别。众所周知，汉字有一些基础的偏旁部首组成，仅有500多个偏旁就组成了超过20000多个汉字。所以作者提出了一种Encoder-Decoder模型对每一个字符的偏旁部首与结构进行预测，并依此对汉字进行重建。
![tupian](F:/Script/Document/paper/figures/01/radical_of_chinese.png)

## 核心知识分解

- zero-shot: 利用语义向量对类外类别进行分类。

## 算法(程序)设计与流程

### Encoder

对输入的包含汉字(单个)的图像利用CNN进行编码，得到特征图像(None, height, weight, channel) reshape to (None, height*weight, channel)

### Decoder

The decoder aims to generate a caption of input Chinese character. The output caption **Y** is represented by a sequence of one-hot encoded words.
$$Y = (y_1,...,y_C)\quad y_i \in R^K$$
Note that, the length of annotation sequence (L) is fixed while the length of caption (C) is variable.

The author take a RNN with attention to generate variable sequences.

#### output one hot vector

$$P(y_t|y_{t-1},X) = g(W_o h(Ey_{t-1}+W_s s_t+ W_c c_t))$$
where *g* denotes a softmax activation function over all the words in the vocabulary, *h* denotes a maxout activation function,

#### GRU

![tupian](F:/Script/Document/paper/figures/01/gru.png)

$f_catt$ denotes the coverage based spatial attention model.

#### Attention

![tupian](F:/Script/Document/paper/figures/01/annotion.png)

## 实验结果

![tupian](F:/Script/Document/paper/figures/01/result.png)

### 注意力机制可视化

![tupian](F:/Script/Document/paper/figures/01/visualize.png)

## 存在的问题

## 改进的思路或方法

本论文是对单个汉字的识别提供了模型，但易于推广到句子的识别情形，需要增加一步对汉字的检测与分割。

可以去查找相关文献寻找汉字的检测与分割方法，不知深度学习中的目标检测算法能否有较好效果。

## 相关链接
