## 显示图像

### 图像数据

您可以将二维数值数组显示为*图像*。在图像中，数组元素决定了图像的亮度或颜色。例如，加载一个图像数组及其颜色图：

```
load durer
whos
Name          Size         Bytes  Class

  X           648x509      2638656  double array
  caption     2x28             112  char array
  map         128x3           3072  double array
```

加载文件 `durer.mat`，向工作区添加三个变量。数组 `X` 是一个 648×509 矩阵，`map` 是作为此图像的颜色图的 128×3 数组。



MAT 文件（例如 `durer.mat`）是用于提供保存 MATLAB® 变量的方法的二进制文件。

`X` 的元素是介于 1 和 128 之间的整数，用作颜色图 `map` 的索引。要显示图像，请使用 `imshow` 函数：

```
imshow(X,map)
```

重新生成阿尔布雷特•丢勒的蚀刻板画。

![img](https://ww2.mathworks.cn/help/matlab/learn_matlab/durer_zh_CN.png)

### 读取和写入图像

使用 [`imread`](https://ww2.mathworks.cn/help/matlab/ref/imread.html) 函数可以读取标准图像文件（TIFF、JPEG、PNG 等）。`imread` 返回的数据类型取决于读取的图像类型。

使用 [`imwrite`](https://ww2.mathworks.cn/help/matlab/ref/imwrite.html) 函数可以将 MATLAB 数据写入到各种标准图像格式。

#### imread

```
A = imread('ngc6543a.jpg');
```

`imread` 返回 650×600×3 数组 `A`。

显示图像。

```
image(A)
```

![Figure contains an axes. The axes contains an object of type image.](https://ww2.mathworks.cn/help/matlab/ref/readanddisplayimageexample_01_zh_CN.png)

#### imwrite

