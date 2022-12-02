## 箭头向量图

### quiver

#### 用法

```matlab
quiver(X,Y,U,V)
quiver(U,V)
quiver(___,scale)
quiver(___,LineSpec)
quiver(___,LineSpec,'filled')
quiver(___,Name,Value)
quiver(ax,___)
q = quiver(___)
```

> quiver(X,Y,U,V) 在由 X 和 Y 指定的笛卡尔坐标上绘制具有定向分量 U 和 V 的箭头。例如，第一个箭头源于点 X(1) 和 Y(1)，按 U(1) 水平延伸，按 V(1) 垂直延伸。默认情况下，quiver 函数缩放箭头长度，使其不重叠。
> quiver(U,V) 在等距点上绘制箭头，箭头的定向分量由 U 和 V 指定。如果 U 和 V 是向量，则箭头的 x 坐标范围是从 1 到 U 和 V 中的元素数，并且 y 坐标均为 1。如果 U 和 V 是矩阵，则箭头的 x 坐标范围是从 1 到 U 和 V 中的列数，箭头的 y 坐标范围是从 1 到 U 和 V 中的行数。
> quiver(___,scale) 调整箭头的长度：当 scale 为正数时，quiver 函数会自动调整箭头的长度，使其不重叠，然后将箭头长度拉伸 scale 倍。例如，scale 为 2 会使箭头长度加倍，scale 为 0.5 会使箭头长度减半。当 scale 为 0 时，如 quiver(X,Y,U,V,0)，则禁用自动缩放。
> quiver(___,LineSpec) 设置线型、标记和颜色。标记出现在由 X 和 Y 指定的点上。如果使用 LineSpec 指定标记，则 quiver 不显示箭尖。要指定标记并显示箭尖，请改为设置 Marker 属性。
> quiver(___,LineSpec,'filled') 填充由 LineSpec 指定的标记。
> quiver(___,Name,Value) 使用一个或多个名称-值对组参数指定箭头图属性。有关属性列表，请参阅 Quiver 属性。在所有其他输入参数之后指定名称-值对组参数。名称-值对组参数应用于箭头图中的所有箭头。
> quiver(ax,___) 在 ax 指定的坐标区中而不是当前坐标区 (gca) 中创建箭头图。参数 ax 可以置于前面的语法中的任何输入参数组合之前。
> q = quiver(___) 返回 Quiver 对象。此对象对于在创建箭头图后控制其属性非常有用。[[1\]](https://zhuanlan.zhihu.com/p/360177013#ref_1)

#### **示例1：基本用法**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 10;
x = linspace(-5, 5, n);
y = linspace(-5, 5, n);
[X Y] = meshgrid(x,y);
U = rand(n);
V = rand(n);
quiver(X, Y, U, V)
```

![img](https://pic3.zhimg.com/80/v2-503e244db6642388631f69cf54a15512_720w.jpg)

这里随机产生数据。

注意，对于二维矢量图，包含4个变量，即坐标X和Y，矢量U和V。并且，在二维坐标系下，这四个都为矩阵。

#### **示例2：箭头大小**

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 5;
x = linspace(-5, 5, n);
y = linspace(-5, 5, n);
[X Y] = meshgrid(x,y);
U = 5*rand(n);
V = 5*rand(n);
subplot(2,2,1)
quiver(X, Y, U, V)
subplot(2,2,2)
quiver(X, Y, U, V, 0)
subplot(2,2,3)
quiver(X, Y, U, V, 0.5)
subplot(2,2,4)
quiver(X, Y, U, V, 3)
xlim([-5, 10])
ylim([-5, 15])
```

![img](https://pic2.zhimg.com/80/v2-99a8d6ab69d73b6d15535e35d0ba80c9_720w.jpg)

可以更改箭头的大小，1是默认情况，系统自动scale到适合的大小，2是真实情况，即箭头大小由U和V分量大小直接确定，3是缩小箭头，4是放大箭头。

#### **示例3：更改图形属性**

例如，可以更改线型、线宽、颜色等属性。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 25;
x = linspace(-5, 5, n);
y = linspace(-5, 5, n);
[X Y] = meshgrid(x,y);
U = sin(X).*sin(Y);
V = cos(Y).*cos(X);
quiver(X, Y, U, V, 'r')
```

![img](https://pic2.zhimg.com/80/v2-caa3e40f8945a71b21bc98ce5ff543e5_720w.jpg)

进一步的，通过句柄对更多属性进行修改，如下所示。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 25;
x = linspace(-5, 5, n);
y = linspace(-5, 5, n);
[X Y] = meshgrid(x,y);
U = sin(X).*sin(Y);
V = cos(Y).*cos(X);
q = quiver(X, Y, U, V);
q.ShowArrowHead = 'off';
q.Marker = 'o';
q.Color = [0 0.5 0];
```

![img](https://pic3.zhimg.com/80/v2-3049a829afdd46cd6987843f4e252972_720w.jpg)

#### **示例4：与等高线图或面图配合，画出更加炫酷的图形**

例如，与等高线图配合

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
spacing = 0.05;
[X,Y] = meshgrid(-2:spacing:2);
Z = X.*exp(-X.^2 - Y.^2)-5;
[DX,DY] = gradient(Z,spacing);
contour(X,Y,Z,10)
spacing = 0.3;
[X,Y] = meshgrid(-2:spacing:2);
Z = X.*exp(-X.^2 - Y.^2)-5;
[DX,DY] = gradient(Z,spacing);
hold on
quiver(X,Y,DX,DY)
axis equal
view(2)
xlim([-2 2])
ylim([-2 2])
```

![img](https://pic1.zhimg.com/80/v2-4ab04164ec1d4f8ea1619c06e18acf80_720w.jpg)

注意，这里 **gradient函数** 用于求取梯度。

又如与面图配合。

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
spacing = 0.1;
[X,Y] = meshgrid(-2:spacing:2);
Z = X.*exp(-X.^2 - Y.^2)-5;
[DX,DY] = gradient(Z,spacing);
surf(X,Y,Z)
shading interp
spacing = 0.3;
[X,Y] = meshgrid(-2:spacing:2);
Z = X.*exp(-X.^2 - Y.^2)-5;
[DX,DY] = gradient(Z,spacing);
hold on
quiver(X,Y,DX,DY,'k')
axis equal
view(2)
xlim([-2 2])
ylim([-2 2])
```

![img](https://pic1.zhimg.com/80/v2-14fb439e58fa2948845c5d9a776899d8_720w.jpg)

注意上述图中，contour和surf函数都是间隔越小越精细，但是又不希望箭头数量过多，因此采用不同间隔，以让图形更加漂亮。

### **quiver3函数**

####  用法

三维箭头向量图quiver3函数用法与quiver函数用法完全一致。

```matlab
quiver3(X,Y,Z,U,V,W)
quiver3(Z,U,V,W)
quiver3(___,scale)
quiver3(___,LineSpec)
quiver3(___,LineSpec,'filled')
quiver3(___,Name,Value)
quiver3(ax,___)
q = quiver3(___)
```

> quiver3(X,Y,Z,U,V,W) 在由 X、Y 和 Z 指定的笛卡尔坐标处，绘制具有定向分量 U、V 和 W 的箭头。例如，第一个箭头源于点 X(1)、Y(1) 和 Z(1)，根据 U(1) 在 x 轴方向延伸，根据 V(1) 在 y 轴方向延伸，并根据 W(1) 在 z 轴方向延伸。默认情况下，quiver3 函数缩放箭头长度，使其不重叠。
> quiver3(Z,U,V,W) 在沿曲面 Z 的等距点上绘制箭头，箭头的定向分量由 U、V 和 W 指定。如果 Z 是向量，则箭头的 x 坐标范围是从 1 到 Z 中的元素数，并且 y 坐标均为 1。如果 Z 是矩阵，则箭头的 x 坐标范围是从 1 到 Z 中的列数，而 y 坐标范围是从 1 到 Z 中的行数。
> quiver3(___,scale) 调整箭头的长度：当 scale 为正数时，quiver3 函数会自动调整箭头的长度，使其不重叠，然后将箭头长度拉伸 scale 倍。例如，scale 为 2 会使箭头长度加倍，scale 为 0.5 会使箭头长度减半。当 scale 为 0 时，如 quiver3(X,Y,Z,U,V,W,0)，则禁用自动缩放。
> quiver3(___,LineSpec) 设置线型、标记和颜色。标记出现在由 X、Y 和 Z 指定的点上。如果使用 LineSpec 指定标记，则 quiver3 不显示箭尖。要指定标记并显示箭尖，请改为设置 Marker 属性。
> quiver3(___,LineSpec,'filled') 填充由 LineSpec 指定的标记。
> quiver3(___,Name,Value) 使用一个或多个名称-值对组参数指定箭头图属性。有关属性列表，请参阅 Quiver 属性。在所有其他输入参数之后指定名称-值对组参数。名称-值对组参数应用于箭头图中的所有箭头。
> quiver3(ax,___) 在 ax 指定的坐标区中而不是当前坐标区 (gca) 中创建箭头图。参数 ax 可以置于前面的语法中的任何输入参数组合之前。
> q = quiver3(___) 返回 Quiver 对象。此对象对于在创建箭头图后控制其属性非常有用。[[2\]](https://zhuanlan.zhihu.com/p/360177013#ref_2)

注意，三维向量图有三个坐标分量，三个数值分量，因此一共六个分量。

#### 示例1

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 10;
x = linspace(-pi/2, pi/2, n);
y = linspace(-pi/2, pi/2, n);
z = linspace(-pi/2, pi/2, 3);
[X Y Z] = meshgrid(x, y, z);
U = sin(X).*sin(Y);
V = cos(Y).*cos(X);
W = sin(X).*cos(Y);
q = quiver3(X, Y, Z, U, V, W);
```

![img](https://pic1.zhimg.com/80/v2-d6d5dc2e50ad77c6a4cc578ea5a42680_720w.jpg)

当然，我们也可以更改属性，例如

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 10;
x = linspace(-pi/2, pi/2, n);
y = linspace(-pi, 0, n);
[X Y] = meshgrid(x, y);
Z = sin(X) + cos(Y);
[U, V, W] = surfnorm(Z);
subplot(4,1,1)
q = quiver3(X, Y, Z, U, V, W);
subplot(4,1,2)
q = quiver3(X, Y, Z, U, V, W, 2);
subplot(4,1,3)
q = quiver3(X, Y, Z, U, V, W, 'color', [0 0.5 0]);
subplot(4,1,4)
q = quiver3(X, Y, Z, U, V, W, 'marker', 'o','color',[0.5 0 0]);
```

![img](https://pic2.zhimg.com/80/v2-6db0b80b7afef42977c104cf5a44cc01_720w.jpg)

注意，这里的 **surfnorm函数** 用于求取面的单位向量，方向向外。

#### 示例2：与surf函数配合

```matlab
clc %更多文章，https://zhuanlan.zhihu.com/p/345799328
clear all
close all
n = 10;
x = linspace(-pi/2, pi/2, n);
y = linspace(-pi, 0, n);
[X Y] = meshgrid(x, y);
Z = sin(X) + cos(Y);
[U V W] = surfnorm(X, Y, Z);
surf(X, Y, Z)
hold on
q = quiver3(X, Y, Z, U, V, W, 1, 'r');
```

![img](https://pic3.zhimg.com/80/v2-c210c885726fd0586df03d739636f3fe_720w.jpg)