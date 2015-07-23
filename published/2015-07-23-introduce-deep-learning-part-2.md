# 深度学习的基础介绍（Part 2）

## 自动编码器

一个自编码基本上就是一个反向神经网络，目的就是为了训练网格来产生一个输入数据的压缩表示。换句话说，即试图产生和输入数据一样的输出数据，但是以某种方式压缩了。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt4OjSfq0Yp4ibXSibxOGBqGXqCL2kIWzbKoesFZAzSZm8MXqS04KGvBo2T6LcfwLI9bWf5V91vwdmQ/640?wx_fmt=png&tp=webp&wxfrom=5)

## 受限波尔兹曼机（RBM）

波尔兹曼机（RBM）是一个通过输入数据学习概率分布的随机生成神经网络。RBM由隐藏层和可视层组成，但与前馈网络不同的是，RBM的可见层和隐藏层之间的连接没有方向性，即数据可以从可见层传送到隐藏层，也可以从隐藏层到可见层。同时，数据之间是完全连接的，即每一个层的每一个单元都和下一层的每个单元连接。如果允许某层的某一单元和任一层连接，那就是无限制的波尔兹曼机，而非受限的波尔兹曼机。标准的RBM有二值的隐藏层和可见层单元组成，即每个激活单元是遵循伯努利分布的0或1，但是也有一些非线性的变量。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt4OjSfq0Yp4ibXSibxOGBqGX8F1KiaPYYAnxEiaDVFsSDOMYBjXkLj9XwVfkJrlfic3ccvw9ib6ib8VTS9A/640?wx_fmt=png&tp=webp&wxfrom=5)

## 深度网络

自动编码器和RBM的隐藏层是很有效的特征探测器，但是这两个方法很多时候却不能直接使用。幸运的是，这些结构可以被叠加来形成深度网络。这些网络可以进一步被逐层训练，解决由传统的反向传播方法产生的梯度消失和数据过分近似的问题。

## 叠加的自动编码器

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt4OjSfq0Yp4ibXSibxOGBqGX5B0c4xqpxBib2mI2CFLRxFf5ZaYiaYndh8fGibIGzZALg2lN2ribpKEtPA/640?wx_fmt=png&tp=webp&wxfrom=5)

叠加的自动编码器就是由一些自动编码器叠加而成的，自动编码器T的隐藏层作为自动编码器T+1的输入层，第一个自动编码器的输入层作为整个网络的输入层。具体的训练程序如下：

1. 用所有可用的训练数据采用反向传播的方法来训练第一个自动编码器（T＝1，或上图中的红色点外加一个输出层）。
2. 训练第二个自动编码器（T＝2，或绿色点）。由于第二个自动编码器的输入层是第一个自动编码器的隐藏层，我们就不再对第一个编码器的输出层感兴趣了，因此可以从网络中去掉。然后，用反向传播的结果来调整权重。
3. 重复上述过程直至所有层都被训练，如上图。
4. 步骤1-3被称作预训练。由于输入数据和输出数据之间没有直接对应，通常会给最后一层（蓝色）加入一个或多个连接的层。整个网络可以被看作是一个多层的感知器，并用反向传播法训练。

## 深度信念网络

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt4OjSfq0Yp4ibXSibxOGBqGXeABuPV69icEl17DWExfdH087icRpapFr42N7rY8jZwNcFTfic1O5UfYjA/640?wx_fmt=png&tp=webp&wxfrom=5)

与自动编码器类似，我们也可以叠加波尔兹曼机来建立深度信念网络（DBN）。第T个RBM的隐藏层作为第T＋1个RBM的可见层，第一个RBM是整个网络的输入层。具体的训练步骤如下：

1. 用对比分歧算法来训练第一个RBM。
2. 训练第二个RBM。由于第二个RBM的可见层是第一个RBM的隐藏层，训练从整合第一个RBM可见层的输入数据开始，然后向其隐藏层传播。这些数据再进一步去进行第二个RBM对比分歧训练。
3. 重复上述过程直至所有层都被训练。
4. 预训练之后网络可以通过将最后一个RBM隐藏层连接到一个或多个完全连接的层之间而进一步扩张。