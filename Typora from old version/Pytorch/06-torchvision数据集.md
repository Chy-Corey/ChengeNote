### Torchvision数据集使用

这里会用到torchvision模块中的数据集，比如CIFAR10，CIFAR10被封装成一个类，我们通过初始化类来获取/加载数据，然后用transformer对图像进行处理。

```python
import torchvision
from torch.utils.tensorboard import SummaryWriter

train_set = torchvision.datasets.CIFAR10("./dataset", True, download=True)
test_set = torchvision.datasets.CIFAR10("./dataset", False, download=True)

dataset_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])

# print(test_set[0])
# print(test_set.classes)
# img, target = test_set[0]
# print(img)
# print(target)
# print(test_set.classes[target])
# img.show()
# print(test_set[0])

writer = SummaryWriter("06")
for i in range(10):
    img, target = test_set[i]
    img_2 = dataset_transform(img)
    writer.add_image("test_set", img_2, i)
writer.close()
```

