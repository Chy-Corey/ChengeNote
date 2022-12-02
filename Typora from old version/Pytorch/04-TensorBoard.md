### TensorBoard

tensorboard是一个可视化工具。

```python
from torch.utils.tensorboard import SummaryWriter
import numpy as np
import os
from PIL import  Image

writer = SummaryWriter("logs")				# 相当于初始化一个白板
root_path = "hymenoptera_data/train"
ants_label_dir = "ants"
bees_label_dir = "bees"
image_name = os.listdir(os.path.join(root_path, ants_label_dir))
for i in range(0,10,1):
    image_path = os.path.join(root_path, ants_label_dir, image_name[i])
    img_PIL = Image.open(image_path)
    img_array = np.array(img_PIL)
    writer.add_image("test", img_array, global_step=i, dataformats='HWC')		# z

for i in range(100):
    writer.add_scalar("y = 2x", 2*i, i)

writer.close()
```

