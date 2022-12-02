## 流线图

**流线图**在流体力学中用的非常多，常和向量箭头图一起，表征流场。

本文讲述采用 **streamline** 和 **streamslice** 函数绘制流线图，其中streamline函数用于绘制**流线**，streamslice函数为**三维可视化切片**。

## 1 streamline函数

**1.1 用法**

```matlab
streamline(X,Y,Z,U,V,W,startx,starty,startz)
streamline(U,V,W,startx,starty,startz)
streamline(XYZ)
streamline(X,Y,U,V,startx,starty)
streamline(U,V,startx,starty)
streamline(XY)
streamline(...,options)
streamline(axes_handle,...)
h = streamline(...)
```

> streamline(X,Y,Z,U,V,W,startx,starty,startz) 根据三维向量数据 U、V 和 W 绘制流线图。数组 X、Y 和 Z 用于定义 U、V 和 W 的坐标，它们必须是单调的，无需间距均匀。X、Y 和 Z 必须具有相同数量的元素，就像由 meshgrid 生成一样。startx、starty 和 startz 定义流线图的起始位置。
> streamline(U,V,W,startx,starty,startz) 假定数组 X、Y 和 Z 定义为 [X,Y,Z] = meshgrid(1:N,1:M,1:P)，其中 [M,N,P] = size(U)。
> streamline(XYZ) 假定 XYZ 是预先计算的顶点数组的元胞数组（由 stream3 生成）。
> streamline(X,Y,U,V,startx,starty) 根据二维向量数据 U、V 绘制流线图。数组 X 和 Y 用于定义 U 和 V 的坐标，它们必须是单调的，无需间距均匀。X 和 Y 必须具有相同数量的元素，就像由 meshgrid 生成一样。startx 和 starty 定义流线图的起始位置。输出参数 h 包含一个线条句柄向量，每个流线图一个句柄。
> streamline(U,V,startx,starty) 假定数组 X 和 Y 定义为 [X,Y] = meshgrid(1:N,1:M)，其中 [M,N] = size(U)。
> streamline(XY) 假定 XY 是预先计算的顶点数组的元胞数组（由 stream2 生成）。
> streamline(...,options) 指定在创建流线图时使用的选项。将 options 定义为一个一元素向量或二元素向量，其中包含步长或步长和一条流线中的最大顶点数。[stepsize]或[stepsize, max_number_vertices]如果未指定值，MATLAB® 将使用默认值：步长 = 0.1（一个元胞的十分之一）最大顶点数 = 1000
> streamline(axes_handle,...) 将图形绘制到句柄为 axes_handle 的坐标区对象中，而不是当前坐标区对象 (gca) 中。
> h = streamline(...) 返回一个线条句柄向量，每个流线图一个句柄。[[1\]](https://zhuanlan.zhihu.com/p/361521042#ref_1)

streamline函数的用法相对其他函数，相对比较复杂。

注意，streamline函数一定要配合quiver函数一起使用，从而便于确定参数范围。

**1.2 示例1**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(0, 10, 15);
y = linspace(0, 10, 15);
[X Y] = meshgrid(x, y);
U = X;
V = Y;
quiver(X, Y, U, V)
xlim([0 10])
ylim([0 10])
```

![img](https://pic2.zhimg.com/80/v2-f4400d358addfc0ee417849b751ec945_720w.jpg)

从这里可以看到整个向量场的大致范围，然后开始添加流线。

这里我们选择添加三条，分别是x=0、y=0和y=x三条流线。首先确定初始值，初始值不能为[0 0]，因为没有方向性，最终选择起始点为

```matlab
startx = [0 0.01 0.01];
starty = [0.01 0.01 0];
```

这样能给定方向，视觉上，流线从原点开始。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(0, 10, 15);
y = linspace(0, 10, 15);
[X Y] = meshgrid(x, y);
U = X;
V = Y;
quiver(X, Y, U, V)
%%
startx = [0 0.01 0.01];
starty = [0.01 0.01 0];
streamline(X, Y, U, V, startx, starty)
xlim([0 10])
ylim([0 10])
```

![img](https://pic2.zhimg.com/80/v2-4574920a8cbc27652c8a15a42db549bd_720w.jpg)

**1.3 示例2**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(0, 20, 15);
y = linspace(0, 20, 15);
[X Y] = meshgrid(x, y);
U = Y;
V = X;
quiver(X, Y, U, V)
startx = [0 0 0 0 0 linspace(4, 19, 5)];
starty = [linspace(4, 19, 5) 0 0 0 0 0];
streamline(X, Y, U, V, startx, starty)
xlim([0 20])
ylim([0 20])
```

![img](https://pic3.zhimg.com/80/v2-2fe6f419bb82ad3e87f3d6e17ad7821a_720w.jpg)

根据对箭头图的观察，这里一共选择10个起点。也可以选择起点不在x或者y轴上，例如选择在某些特定线上，例如

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(1, 20, 15);
y = linspace(1, 20, 15);
[X Y] = meshgrid(x, y);
U = Y;
V = X;
quiver(X, Y, U, V)
startx = linspace(1, 18, 5);
starty = -startx/2 + 10;
streamline(X, Y, U, V, startx, starty)
xlim([0 20])
ylim([0 20])
```

![img](https://pic2.zhimg.com/80/v2-0aae85ac4ebddfe071f6f7a435c555f5_720w.jpg)

这里选择将起点放在y = -2x + 10这条直线上。

**1.4 示例3 ：属性更改**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(1, 20, 8);
y = linspace(1, 20, 8);
[X Y] = meshgrid(x, y);
U = X;
V = -Y;
quiver(X, Y, U, V, 'Color', [0.3 0.3 0.3])
startx = linspace(1, 19, 8);
starty = 20*ones(length(startx), 1);
s = streamline(X, Y, U, V, startx, starty)
for i = 1:length(startx)
    s(i).Color = [0.8 0 0];
    s(i).LineStyle = '--';
    s(i).LineWidth = 1;
end
xlim([-0 20])
ylim([-0 20])
```

![img](https://pic1.zhimg.com/80/v2-3ed73a204d29ca4791633fe15f824600_720w.jpg)

可以通过句柄对图形属性进行修改。注意，句柄

```matlab
s = streamline(X, Y, U, V, startx, starty)
```

返回的是一个line数据，里面包含所有的线。可以通过

```matlab
s(i)
```

修改某一个流线，可参考[箭头图feather和compass函数](https://zhuanlan.zhihu.com/p/360837773)中的设置，也可以使用for循环，直接对所有线条进行统一修改。甚至可以做成渐变色，例如

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(1, 20, 8);
y = linspace(1, 20, 8);
[X Y] = meshgrid(x, y);
U = X;
V = -Y;
quiver(X, Y, U, V, 'Color', [0.3 0.3 0.3])
startx = linspace(1, 19, 8);
starty = 20*ones(length(startx), 1);
s = streamline(X, Y, U, V, startx, starty)
for i = 1:length(startx)
    s(i).Color = 0.1*[8-i 0 0] + [0.2 0 0];
    s(i).LineStyle = '--';
    s(i).LineWidth = 1;
end
xlim([-0 20])
ylim([-0 20])
```

![img](https://pic3.zhimg.com/80/v2-52df422f9e9eacef6fe2cb72dfb356ca_720w.jpg)

重点是

```matlab
s(i).Color = 0.1*[8-i 0 0] + [0.2 0 0];
```

这条命宁。

三维流线画图方法和二维一致，但为了可视化，一般不采用三维流线图，而是采用切片函数 streamslice绘制。

## 2 streamslice函数

**2.1 用法**

```matlab
streamslice(X,Y,Z,U,V,W,startx,starty,startz)
streamslice(U,V,W,startx,starty,startz)
streamslice(X,Y,U,V)
streamslice(U,V)
streamslice(...,density)
streamslice(...,'arrowsmode')
streamslice(...,'method')
streamslice(axes_handle,...)
h = streamslice(...)
[vertices arrowvertices] = streamslice(...)
```

> streamslice(X,Y,Z,U,V,W,startx,starty,startz) 在与轴对齐的 x、y、z 平面中根据向量数据 U、V、W 绘制间隔合适的流线图（具有方向箭头），流线图以向量 startx、starty、startz 中的点为起始点。数组 X、Y 和 Z 用于定义 U、V 和 W 的坐标，它们必须是单调的，无需间距均匀。X、Y 和 Z 必须具有相同数量的元素，就像由 meshgrid 生成一样。U、V 和 W 必须是 m×n×p 三维体数组。不要假定流动情况与切片平面平行。例如，在常量值 z 位置的流切片中，当计算该平面的流线图时，向量场 W 的 z 分量将被忽略。流切片可用于确定开始绘制流线图、流管和流带的位置。
> streamslice(U,V,W,startx,starty,startz) 假定 X、Y 和 Z 由以下表达式确定 [X,Y,Z] = meshgrid(1:n,1:m,1:p) 其中 [m,n,p] = size(U)。
> streamslice(X,Y,U,V) 根据向量三维体数据 U、V 绘制间距合适的流线图（带方向箭头）。数组 X 和 Y 用于定义 U 和 V 的坐标，它们必须是单调的，无需间距均匀。X 和 Y 必须具有相同数量的元素，就像由 meshgrid 生成一样。
> streamslice(U,V) 假定 X、Y 和 Z 由以下表达式确定 [X,Y,Z] = meshgrid(1:n,1:m,1:p)其中 [m,n,p] = size(U)。
> streamslice(...,density) 修改流线图的自动间距设置。density 必须大于 0。默认值是 1；值越大在每个平面上生成的流线图越多。例如，2 生成大约两倍的流线图，而 0.5 生成大约一半的流线图。
> streamslice(...,'arrowsmode') 确定是否具有方向箭头。arrowmode 可以是 arrows - 在流线图上绘制指向箭头（默认值）。noarrows - 不绘制指向箭头。
> streamslice(...,'method') 指定要使用的插值方法。method 可以是 linear - 线性插值（默认值）cubic - 三次插值 nearest - 最近邻点插值 有关插值方法的详细信息，请参阅 interp3。
> streamslice(axes_handle,...) 将图形绘制到句柄为 axes_handle 的坐标区对象中，而不是当前坐标区对象 (gca) 中。
> h = streamslice(...) 返回所创建的线条对象的句柄向量。
> [vertices arrowvertices] = streamslice(...) 返回用于绘制流线图和箭头的两个顶点元胞数组。您可以将这些值传递给任何流线图绘制函数（streamline、streamribbon、streamtube）。[[2\]](https://zhuanlan.zhihu.com/p/361521042#ref_2)

从上面可以看出，streamslice函数的用法非常多，而且复杂，然而MATLAB的帮助文档给出的示例很少。

**1.2 示例1**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(1, 20, 20);
y = linspace(1, 20, 20);
z = linspace(1, 20, 20);
[X Y Z] = meshgrid(x, y, z);
U = X;
V = -Y;
W = Z;
startx = [];
starty = [];
startz = [10];
s = streamslice(X, Y, Z, U, V, W, startx, starty, startz);
```

![img](https://pic4.zhimg.com/80/v2-9ca3eb91179ea30e3c8d9d952a0d9fdf_720w.jpg)

图中箭头是默认的，代表方向。

这里在z = 10的位置做切片，也可以在任意位置做切片，例如在x、y和z平面都做切面。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
x = linspace(1, 20, 20);
y = linspace(1, 20, 20);
z = linspace(1, 20, 20);
[X Y Z] = meshgrid(x, y, z);
U = X;
V = -Y;
W = Z;
startx = [18];
starty = [10];
startz = [2];
s = streamslice(X, Y, Z, U, V, W, startx, starty, startz);
view(3)
```

![img](https://pic3.zhimg.com/80/v2-288466c9445e03ccc2464439d3ba0e92_720w.jpg)

也可以随机定义面，并在面上做切片，具体可以参考[三维等高线图contourslice切片函数](https://zhuanlan.zhihu.com/p/360092746)，两者用法非常相似。

**1.3 示例2：风场**

下面这个例子是MATLAB自带的，是风场的数据。

调用方法为

```matlab
load wind
```

wind数据已存放在MATLAB的示例库中，注意调用得到的坐标和速度等数据都是小写字母。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
load wind
startx = [0];
starty = [0];
startz = [2];
s = streamslice(x, y, z, u, v, w, startx, starty, startz);
axis tight %让坐标轴与数据边界贴合
```

![img](https://pic1.zhimg.com/80/v2-f302601bf64d509e6393f316d534af00_720w.jpg)

通过

```matlab
s =streamslice(x, y, z, u, v, w, startx, starty, startz, density);
```

![img](https://pic1.zhimg.com/80/v2-126cd43b46620610bf21cfa4718eb468_720w.jpg)density = 0.1

![img](https://pic3.zhimg.com/80/v2-52bb3f6b988c7c39090efbfd79a628ee_720w.jpg)density = 3

命宁可以调节流线的密度，默认值是1。将值设定为0.1 和 3 的效果如上两图所示。值越小，密度越低，值越大，密度越高。

**1.4 示例3：箭头**

默认绘制是带箭头的，这里可以取消箭头绘制，例如

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
load wind
startx = [0];
starty = [0];
startz = [2];
density = 1;
s = streamslice(x, y, z, u, v, w, startx, starty, startz, density, 'noarrows');
axis tight
```

![img](https://pic3.zhimg.com/80/v2-34921fb599f04c5d1413402de37fb5e6_720w.jpg)

在属性中指定为 'noarrows' 可隐去箭头。但是不推荐这样做，没有箭头难以判断方向。

**1.5 示例4：句柄控制**

通过

```matlab
[s] = streamslice(x, y, z, u, v, w, startx, starty, startz, density);
```

可以返回句柄，其中s为所有线的参数，包括流线和箭头。而

```matlab
[s, a] = streamslice(x, y, z, u, v, w, startx, starty, startz, density);
```

返回的数据中，s为流线，a为所有箭头参数。可以很容易通过句柄对线和箭头参数进行修改，例如

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
load wind
startx = [0];
starty = [0];
startz = [2];
density = 0.3;
[s] = streamslice(x, y, z, u, v, w, startx, starty, startz, density);
axis tight
for i = 1:length(s)
    s(i).LineWidth = 1;
    s(i).Color = [rand(1) 0 0];
end
```

![img](https://pic2.zhimg.com/80/v2-eaf7a63de83cda3bfc4654ef5d5dd291_720w.jpg)

这里将线颜色设置为随机渐变红色。更多用法可以查看句柄中的属性，可参考[MATLAB画图技巧与实例（二十四）](https://zhuanlan.zhihu.com/p/360837773)。

https://zhuanlan.zhihu.com/p/312069817)



## 参考

1. [^](https://zhuanlan.zhihu.com/p/361521042#ref_1_0)MATLAB帮助文档 https://ww2.mathworks.cn/help/matlab/ref/streamline.html
2. [^](https://zhuanlan.zhihu.com/p/361521042#ref_2_0)MATLAB https://ww2.mathworks.cn/help/matlab/ref/streamslice.html