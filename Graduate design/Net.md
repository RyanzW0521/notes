# About the Nets

### SegNet

SegNet具有编码器网络和相应的解码器网络，接着是按最终像素的分类层。
1. * Encoder编码器
	在编码器处，执行卷积和最大池化。
	VGG-16有13个卷积层。 （不用全连接的层），在进行2×2最大池化时，存储相应的最大池化索引（位置）。
	* Decoder解码器
		使用最大池化的索引进行上采样
		在解码器处，执行上采样和卷积。最后，每个像素送到softmax分类器。
		在上采样期间，如上所示，调用相应编码器层处的最大池化索引以进行上采样。
	最后，使用K类softmax分类器来预测每个像素的类别。
2. DeconvNet 和U-Net的不同

	DeconvNet和U-Net具有与SegNet类似的结构。
Similar upsampling approach called unpooling is used.使用了类似的上采样方法，称为unpooling 反池化。
不同，有完全连接的层，这使模型规模更大。

3.  U-Net 与 SegNet不同之处

	* 用于生物医学图像分割。
	* 整个特征映射不是使用池化索引，而是从编码器传输到解码器，然后使用concatenation串联来执行卷积。
	* 这使模型更大，需要更多内存

### ResNet
> 背景：**深度网络的退化**：从经验来看，网络的深度对模型的性能至关重要，当增加网络层数后，网络可以进行更加复杂的特征模式的提取，所以当模型更深时理论上可以取得更好的结果，但是更深的网络其性能一定会更好吗？实验发现深度网络出现了退化问题（Degradation problem）：网络深度增加时，网络准确度出现饱和，甚至出现下降。56层的网络可能比20层网络效果还要差。这不会是过拟合问题，因为56层网络的训练误差同样高。我们知道深层网络存在着**梯度消失**或者爆炸的问题，这使得深度学习模型很难训练。但是现在已经存在一些技术手段如BatchNorm来缓解这个问题。因此，出现深度网络的退化问题是非常令人诧异的。

* 对于一个堆积层结构（几层堆积而成）当输入为x时其学习到的特征记为H(x)，现在我们希望其可以学习到残差F(x)=H(x)-x，这样其实原始的学习特征是F(x)+x。之所以这样是因为残差学习相比原始特征直接学习更容易。当残差为0时，此时堆积层仅仅做了恒等映射，至少网络性能不会下降，实际上残差不会为0，这也会使得堆积层在输入特征基础上学习到新的特征，从而拥有更好的性能。这有点类似与电路中的“短路”，所以是一种短路连接（shortcut connection）。
![figure1](https://pic4.zhimg.com/80/v2-252e6d9979a2a91c2d3033b9b73eb69f_1440w.jpg)

* 利用链式规则，可以求得反向过程的梯度：
![figure2](https://www.zhihu.com/equation?tex=%5Cfrac%7B%5Cpartial+loss%7D%7B%5Cpartial+%7B%7Bx%7D_%7Bl%7D%7D%7D%3D%5Cfrac%7B%5Cpartial+loss%7D%7B%5Cpartial+%7B%7Bx%7D_%7BL%7D%7D%7D%5Ccdot+%5Cfrac%7B%5Cpartial+%7B%7Bx%7D_%7BL%7D%7D%7D%7B%5Cpartial+%7B%7Bx%7D_%7Bl%7D%7D%7D%3D%5Cfrac%7B%5Cpartial+loss%7D%7B%5Cpartial+%7B%7Bx%7D_%7BL%7D%7D%7D%5Ccdot+%5Cleft%28+1%2B%5Cfrac%7B%5Cpartial+%7D%7B%5Cpartial+%7B%7Bx%7D_%7BL%7D%7D%7D%5Csum%5Climits_%7Bi%3Dl%7D%5E%7BL-1%7D%7BF%28%7B%7Bx%7D_%7Bi%7D%7D%2C%7B%7BW%7D_%7Bi%7D%7D%29%7D+%5Cright%29)
	式子的第一个因子表示的损失函数到达L 的梯度，小括号中的1表明短路机制可以无损地传播梯度，而另外一项残差梯度则需要经过带有weights的层，梯度不是直接传递过来的。残差梯度不会那么巧全为-1，而且就算其比较小，有1的存在也不会导致梯度消失。所以残差学习会更容易。要注意上面的推导并不是严格的证明。
##### ResNet的结构
* ResNet网络是参考了VGG19网络，在其基础上进行了修改，并通过短路机制加入了残差单元，如图5所示。变化主要体现在ResNet直接使用stride=2的卷积做下采样，并且用global average pool层替换了全连接层。ResNet的一个重要设计原则是：当feature map大小降低一半时，feature map的数量增加一倍，这保持了网络层的复杂度。从图5中可以看到，ResNet相比普通网络每两层间增加了短路机制，这就形成了残差学习，其中虚线表示feature map数量发生了改变。图5展示的34-layer的ResNet，还可以构建更深的网络如表1所示。从表中可以看到，对于18-layer和34-layer的ResNet，其进行的两层间的残差学习，当网络更深时，其进行的是三层间的残差学习，三层卷积核分别是1x1，3x3和1x1，一个值得注意的是隐含层的feature map数量是比较小的，并且是输出feature map数量的1/4。
![figure3](https://pic2.zhimg.com/80/v2-7cb9c03871ab1faa7ca23199ac403bd9_1440w.jpg)
* 对于短路连接，当输入和输出维度一致时，可以直接将输入加到输出上。但是当维度不一致时（对应的是维度增加一倍），这就不能直接相加。有两种策略：（1）采用zero-padding增加维度，此时一般要先做一个downsamp，可以采用strde=2的pooling，这样不会增加参数；（2）采用新的映射（projection shortcut），一般采用1x1的卷积，这样会增加参数，也会增加计算量。短路连接除了直接使用恒等映射，当然都可以采用projection shortcut。
![figure4](https://pic1.zhimg.com/v2-4e0bf37ecad2f306fe09d32a2d37d908_r.jpg)
