### Pytorch加载数据



在torch中加载数据主要使用两个类，分别为Dataset 和 Dataloader。

Dataset提供一种方式获取数据及其label

Dataloader为后面的网络提供不同的数据形式

使用这两个类时，可以输入：

```python
from torch.utils.data import Dataset
from torch.utils.data import DataLoader
help(Dataset)
help(DataLoader)
```

最好在jupyter输入，这样更直观