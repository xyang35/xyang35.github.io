---
layout: post
comments: true
title: "GAN入门实践（二）--Pytorch实现"
excerpt: "用Pytorch实现一个基于生成对抗模型的高斯分布生成器。"
date: 2017-08-26
mathjax: false
---

最近在网上看到一个据说是 Alex Smola 写的关于**生成对抗网络**（Generative Adversarial Network, GAN）的入门教程，目的是从实践的角度讲解 GAN 的基本思想和实现过程。这个教程没有采用最普遍的用 GAN 生成图像作为例子，而是用生成一个特定的高斯分布作为切入点来讲解 GAN 的工作原理和效果。对 GAN 感兴趣的朋友不妨去看看这个简短的教程，直观易懂而不失有趣，链接如下：

> [https://github.com/zackchase/mxnet-the-straight-dope/blob/master/P10-C01-gan-intro.ipynb](https://github.com/zackchase/mxnet-the-straight-dope/blob/master/P10-C01-gan-intro.ipynb)

这个教程的实现是基于 mxnet 的，而我最近比较常用的是 tensorflow 和 pytorch，因此花了点小时间把其中的内容用这两个框架实现了一遍。一方面是加深自己对 GAN 和 tensorflow ／ pytorch 的理解，同时也给感兴趣的朋友多一些参考。由于教程已经非常通俗易懂了，本文不会再重复其中的讲解，而是记录一些实现过程中个人觉得要注意的地方。

***Pytorch实现的全代码在[这里](https://github.com/xyang35/Introduction-to-GAN/blob/master/GAN-pytorch.ipynb)***。
其中还参考了[CycleGAN](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)的Pytorch实现和[DCGAN](https://github.com/pytorch/examples/tree/master/dcgan)的Pytorch版本实现。

Tensorflow篇请看[这里](https://xyang35.github.io/2017/08/22/GAN-1/)。


----------

### 模型定义

```
class GAN(object):
    def __init__(self):

        # define the network
        self.netG = nn.Linear(2, 2)
        self.netD = nn.Sequential(
                        nn.Linear(2, 5), nn.Tanh(),
                        nn.Linear(5, 3), nn.Tanh(),
                        nn.Linear(3, 2))
        
        # define loss function
        self.criterion = nn.CrossEntropyLoss()
```

Pytorch 里面的函数调用比 Tensorflow 清晰很多，对初学者来说更容易接受。这里我们直接用 nn.Linear() 定义简单的全连接网络，并且用 nn.Sequential() 将多层网络拼接起来。

### 定义前向传播

```
    def forward(self, x, z):
        self.z = Variable((torch.from_numpy(z)).float())
        self.fake_x = self.netG(self.z)
        self.real_x = Variable((torch.from_numpy(x)).float())
        
        self.label = Variable(torch.LongTensor(x.shape[0]).fill_(1), requires_grad=False)    # define null label with specific size     
```

Pytorch里对前向和后向的传播都可以方便地进行细致的定义。这里要注意的是**网络的输入输出是一个变量**（Variable），而定义一个变量需要 torch.Tensor。上面的代码提供了一种从 numpy 矩阵转化为 Tensor 的方法，并且将类型转为 FloatTensor（因为 nn.Linear() 需要）。另外，我们还定义了一个 label 变量，为了方便之后计算损失函数，因为损失函数的输入也是需要是变量。由于 label 变量是**不需要计算梯度**的，我们将 requires_grad 设为 False，并且为了符合 CrossEntropyLoss() 的要求使用 LongTensor 类型。

### 定义后向传播

```
    def backward_D(self):
        # (1) Update D network: maximize log(D(x)) + log(1 - D(G(z)))
        pred_fake = self.netD(self.fake_x.detach())    # stop backprop to the generator by detaching fake_B
        self.loss_D_fake = self.criterion(pred_fake, self.label*0)

        pred_real = self.netD(self.real_x)
        self.loss_D_real = self.criterion(pred_real, self.label*1)
        
        self.loss_D = self.loss_D_fake + self.loss_D_real
        
        self.loss_D.backward()
    
    def backward_G(self):
        # (2) Update G network: maximize log(D(G(z)))
        pred_fake = self.netD(self.fake_x)
        self.loss_G = self.criterion(pred_fake, self.label*1)    # fool the discriminator
        
        self.loss_G.backward()
```

这两个函数定义了损失函数是如何计算以及梯度是如何反向传播的。注意的是判别器的反向传播函数中用到了 detach()，这是为了避免梯度反向传播到生成器中，因为这里我们只更新判别器的梯度。不然的话，调用 loss_D.backward() 会导致生成器中变量的 grad 发生变化。定义好各种损失函数后，调用 backward() 函数就可以实现自动求各个变量的梯度，十分方便。


### 定义优化

```
d_optim = torch.optim.Adam(gan.netD.parameters(), lr=0.05)
g_optim = torch.optim.Adam(gan.netG.parameters(), lr=0.01)
```
虽然上面定义了梯度是怎么计算的，但具体怎么使用这些梯度进行权值的更新则是通过torch.optim 中的优化器来定义。其中第一个输入参数为该优化器要优化的变量。

### 模型初始化

```
# initialize weights in the model
def init_weights(m):
    if type(m) == nn.Linear:
        m.weight.data.normal_(0.0, 0.02)

gan.netD.apply(init_weights)
gan.netG.apply(init_weights)
```

Pytorch中将 nn 中的*网络层定义与初始化分开*了，通过上面的方法可以对网络的变量进行初始化。

### 模型训练

```
	......
	
        # forward
        gan.forward(x_batch, z_batch)
            
        # update D network
        d_optim.zero_grad()
        gan.backward_D()
        d_optim.step()
        
        # update G network
        g_optim.zero_grad()
        gan.backward_G()
        g_optim.step()
```

这里唯一需要注意的是，在计算反向传播前，要先对变量的梯度进行清零，使用 zero_grad() 函数即可。然后使用 step() 执行变量的更新。

### 模型测试

```
    def test(self, x, z):
        z = Variable((torch.from_numpy(z)).float(), volatile=True)
        fake_x = self.netG(z)
        real_x = Variable((torch.from_numpy(x)).float(), volatile=True)
        
        pred_fake = self.netD(fake_x)
        pred_real = self.netD(real_x)
        
        return fake_x.data.numpy(), pred_real.data.numpy(), pred_fake.data.numpy()
```

其实前面 GAN 类中还定义了一个用于测试的函数。这里需要注意的是，因为变量都不需要求梯度（不需要训练）了，所以我们把 volatile 设为 True。上面代码还给出了一个例子如何将 Variable 转化为 numpy 矩阵。