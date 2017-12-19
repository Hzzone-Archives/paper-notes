### Caffe自定义Python Layer
由于Data层和loss层不需要gpu优化，而且有时候有需要定制输入（multi-label分类）或者损失函数（自定义损失函数），所以可以用Caffe的Python Layer实现。如果需要定制卷积层，全连接层或者其他需要gpu优化的层，最好用C++。      
自定义类直接继承的是`caffe.Layer`，必须重写`setup()`，`reshape()`，`forward()`，`backward()`函数，其他的函数或者属性可以自己定义，没有限制。      
`top`是下一层，`bottom`是上一层，是一个list，所以可以检查上一层的个数
* `setup()`是类启动时该做的事情，比如层所需数据的初始化。 
* `reshape()`就是取数据然后把它规范化为四维的矩阵。每次取数据都会调用此函数。 
* `forward()`就是网络的前向运行，这里就是把取到的数据往前传递，因为没有其他运算。 
* `backward()`就是网络的反馈，data层是没有反馈的，所以这里就直接pass。

`bottom[0].data`，`top[0].data`取这一层的数据，`bottom[0].num`取这一层的输出个数，比如神经元个数。