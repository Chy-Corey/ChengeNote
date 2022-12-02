### Transforms

Transforms是一个python工具箱，用来对图像进行处理。Transforms文件中有一个Totensor类，可以把图像类型转为tensor类，至于为什么要用tensor类来表示图像，主要原因是：tensor类的图像自带神经网络需要的属性，它会给每个图像封装比如梯度、梯度函数、传递函数等属性。

```python
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms
from PIL import Image

img_path = "data/train/ants_image/0013035.jpg"
img = Image.open(img_path)
writer = SummaryWriter("logs")
tensor_trans = transforms.ToTensor()
tensor_img = tensor_trans(img)
writer.add_image("Tensor_img",tensor_img)
writer.close()
```

#### Torch.reshape()

torch.reshape()是如何操作的

问题背景：假设当我们的dataloader的batch_size设置为64。并且经过卷积（out_channels=6）之后，我们需要使用tensorboard可视化，而彩色图片的writer.add.images(output)的彩色图片是in_channels=3的。

那么则需要对卷积后的图片进行reshape

Torch.size(64,6,30,30)---->torch.size(-1,3,30,30)

-1的意思为最后自动计算其batch_size

输出通道就是有多少个卷积核，同一个卷积核得到的数据叠成一个通道，但由于减少了三个通道，从而每一个通道的数量增加。
因而结果为torch.size(128,3,30,30)
