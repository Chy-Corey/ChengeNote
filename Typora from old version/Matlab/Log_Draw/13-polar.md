## 极坐标图

从本文起，讲述极坐标绘图。在科研中，极坐标绘图比较常见。

在MATLAB中，一共有五个极坐标绘图函数，分别是绘制**点和线条、散点、直方图、矢量和函数绘图**，如下所示：

- polarplot 在极坐标中绘制线条，对应plot函数
- polarscatter 极坐标中的散点图，对应scatter函数
- polarhistogram 极坐标中的直方图，对应histogram函数
- compass 绘制从原点发射出的箭头，矢量绘图函数
- ezpolar 易用的极坐标绘图函数，对应fplot和ezplot函数

本文主要讲述polarplot函数。

## 1 polarplot函数

**1.1 用法**

```matlab
polarplot(theta,rho)
polarplot(theta,rho,LineSpec)
polarplot(theta1,rho1,...,thetaN,rhoN)
polarplot(theta1,rho1,LineSpec1,...,thetaN,rhoN,LineSpecN)
polarplot(rho)
polarplot(rho,LineSpec)
polarplot(Z)
polarplot(Z,LineSpec)
polarplot(___,Name,Value)
polarplot(pax,___)
p = polarplot(___)
```

> polarplot(theta,rho) 在极坐标中绘制线条，由 theta 表示弧度角，rho 表示每个点的半径值。输入必须是长度相等的向量或大小相等的矩阵。如果输入为矩阵，polarplot 将绘制 rho 的列对 theta 的列的图。也可以一个输入为向量，另一个为矩阵，但向量的长度必须与矩阵的一个维度相等。
> polarplot(theta,rho,LineSpec) 设置线条的线型、标记符号和颜色。
> polarplot(theta1,rho1,...,thetaN,rhoN) 绘制多个 rho,theta 对组。
> polarplot(theta1,rho1,LineSpec1,...,thetaN,rhoN,LineSpecN) 指定每个线条的线型、标记符号和颜色。
> polarplot(rho) 按等间隔角度（介于 0 和 2π 之间）绘制 rho 中的半径值。
> polarplot(rho,LineSpec) 设置线条的线型、标记符号和颜色。
> polarplot(Z) 绘制 Z 中的复数值。
> polarplot(Z,LineSpec) 设置线条的线型、标记符号和颜色。
> polarplot(___,Name,Value) 使用一个或多个 Name,Value 对组参数指定图形线条的属性。属性设置适用于所有线条。无法使用 Name,Value 对组为不同的线条指定不同的属性值。
> polarplot(pax,___) 使用 pax 指定的 PolarAxes 对象，而不是使用当前坐标区。
> p = polarplot(___) 返回一个或多个图形线条对象。在创建图形线条对象之后使用 p 为其设置属性。有关属性列表，请参阅 Line 属性。[[1\]](https://zhuanlan.zhihu.com/p/346617684#ref_1)

用法和plot函数一模一样，参见[（一）](https://zhuanlan.zhihu.com/p/312069817)。

**1.2 示例：蝴蝶**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.05:2*pi;
r = abs(sqrt(sin(t).^5 + cos(t).^5));
subplot(1,2,1)
polarplot(t,r,'-','linewidth',1)

subplot(1,2,2)
polarplot(t,r,'ro--')
```

![img](https://pic1.zhimg.com/80/v2-49066d1272cb51e209e012c6652b83a0_720w.jpg)

与plot函数一样，我们可以对点和线条进行自定设置，常用包括：

- Color 线色
- LineStyle 线型
- LineWidth 线宽
- Marker 点型
- MarkerEdgeColor 点边缘颜色
- MarkerFaceColor 点内部填充颜色
- MarkerSize 点大小

**可选择范围如下**

| 线型      | 颜色   | 符号     | 符号     |
| --------- | ------ | -------- | -------- |
| - 实线    | b 蓝色 | . 点     | ^ 上三角 |
| : 虚线,   | g 绿色 | o 圆圈   | < 左三角 |
| -. 点划线 | r 红色 | × 叉号   | > 右三角 |
| -- 虚线   | c 青色 | + 加号   | v 下三角 |
|           | m 品红 | * 星号   | h 六角星 |
|           | y 黄色 | s 方块   |          |
|           | k 黑色 | d 菱形   |          |
|           | w 白色 | p 五角星 |          |
|           |        | _ 横线   |          |
|           |        | \| 竖线  |          |

**注意**：在MATLABR2020b中，在原有基础上，增加了 '_' 和 '|' 两个新的点类型。

可以直接在

```matlab
polarplot(t,r,'-','linewidth',1)
```

绘图函数中进行设置，也可以通过句柄

```matlab
p = polarplot(t,r);
p.LineWidth = 1;
p.LineStyle = '-';
```

进行设置。**注意**，直接设置时，MATLAB不区分大小写，但在句柄设置时，必须注意大小写。

**1.3 示例3：绘制多个线条，太极图**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.1:3*pi;
r = 4*sqrt(t);
polarplot(t,r,'o','linewidth',2,'Color',[0.1 0.5 0.9])
r = -4*sqrt(t);
hold on
polarplot(t,r,'s','linewidth',2,'Color',[0.9 0.1 0.5])
```

![img](https://pic1.zhimg.com/80/v2-3aaefe6112d36023e3831297024bfb54_720w.jpg)

polarplot函数和plot函数一样，可以在同一张图中，绘制多个函数，如上图所示。

## 2 常见的极坐标图形

除了polarplot函数的用法，这里展示一些常见的有趣的极坐标图形。除了上面的蝴蝶图、太极图外，还有很多。

**2.1 心形图**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.01:2*pi;
r = 1 - sin(t);
polarplot(t,r,'r-','linewidth',1)
```

![img](https://pic4.zhimg.com/80/v2-f6345492d5799e2b496f782c3d0a7273_720w.jpg)

**2.2 三叶草**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.01:2*pi;
r = cos(3*t);
polarplot(t,r,'r-','linewidth',2)
```

![img](https://pic2.zhimg.com/80/v2-79505e1ccaf94105840eb935fbddfa81_720w.jpg)

**2.3 四叶草**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.01:2*pi;
r = sin(t) .* cos(t);
polarplot(t,r,'r-','linewidth',2)
```

![img](https://pic3.zhimg.com/80/v2-cbe31227e8543d4d31987b7c5b6a6c9a_720w.jpg)

**2.4 八叶草**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
t = 0:0.01:2*pi;
r = sin(2*t) .* cos(2*t);
polarplot(t,r,'r-','linewidth',1)
```

![img](https://pic2.zhimg.com/80/v2-5cd80bc31ac24af2f91561bbd0464281_720w.jpg)

**2.5 弹簧**

```matlab
t = 0:0.01:2*pi;
r = 1 + 0.15*sin(t*40);
polarplot(t,r,'r-','linewidth',1)
```

![img](https://pic4.zhimg.com/80/v2-69ef3fdf95a17d3ca506439794f68db7_720w.jpg)

polarscatter函数，专门用于绘制**极坐标系下的散点图**，用法、原理与直角坐标系散点图函数scatter用法一致，可参见[散点图scatter和scatter3函数](https://zhuanlan.zhihu.com/p/335615201)。

在科研中，散点图用途也非常广泛。与plot函数相比，散点图能表达更多信息。

例如，plot函数一般只能表现3个维度信息，点坐标表示x和y维度，不同颜色、线型或者点型表示不同case。

但散点图可以表达更多信息，其点的**颜色和大小可以受输入参数控制**。

注意，polarplot函数在R2016b版本开始推出。

## 3 polarscatter函数

**3.1 用法**

```matlab
polarscatter(th,r)
polarscatter(th,r,sz)
polarscatter(th,r,sz,c)
polarscatter(___,mkr)
polarscatter(___,'filled')
polarscatter(___,Name,Value)
polarscatter(pax,___)
ps = polarscatter(___)
```

> polarscatter(th,r) 绘制 th 对 r 的图，并在每个数据点显示一个圆圈。th 和 r 必须是具有相同长度的向量。必须以弧度为单位指定 th。
> polarscatter(th,r,sz) 设置标记大小，其中 sz 以平方磅为单位指定每个标记的面积。要以相同的大小绘制所有标记，请将 sz 指定为标量。要以不同的大小绘制标记，请将 sz 指定为长度与 th 相同的向量。
> polarscatter(th,r,sz,c) 设置标记颜色，其中 c 是向量、三列矩阵、RGB 三元组或颜色名称，例如 'red'。
> polarscatter(___,mkr) 设置标记符号。例如，'+' 显示十字标记。在上述语法中的任何输入参数组合之后指定标记符号。
> polarscatter(___,'filled') 填充标记内部。
> polarscatter(___,Name,Value) 使用一个或多个名称-值对组参数修改散点图的外观。例如，您可以指定 'FaceAlpha' 和一个介于 0 和 1 之间的标量值，从而使用半透明标记。
> polarscatter(pax,___) 将在 pax 指定的极坐标区（而不是当前坐标区）中绘制图形。
> ps = polarscatter(___) 返回 Scatter 对象。在创建 Scatter 对象之后可使用 ps 修改其外观。有关属性列表，请参阅 Scatter 属性。[[1\]](https://zhuanlan.zhihu.com/p/346854919#ref_1)

用法和scatter函数一样，注意顺序，依次是

```matlab
theat值、r值、点大小、点颜色、点型、是否填充、其他属性。
```

顺序错了可能会报错。

**3.2 示例1：基本功能**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
num = 50;
theat = rand(num,1)*2*pi;
r = rand(num,1);
subplot(1,2,1)
polarscatter(theat,r);
subplot(1,2,2)
polarscatter(theat,r,'s','filled');
```

![img](https://pic3.zhimg.com/80/v2-1a562fc017e725f326a616af65d4cb0a_720w.jpg)

这里用随机函数产生一些随机值。左图为默认情况，可以**更改点型和选择是否填充**。

注意，这些图的效果暂时和polarplot函数一致。

**3.3 示例2：点大小随输入参数变化**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
num = 100;
theat = rand(num,1)*2*pi;
r = rand(num,1);
s = r*300;
polarscatter(theat,r,s,'filled');
```

![img](https://pic4.zhimg.com/80/v2-1996374fde016afd535bc4414edeb597_720w.jpg)

散点图可以让**点的大小随位置发生变化**，如上图所示，因此可以多表现一个维度信息。

**3.4 示例3：点颜色随输入参数变化**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
num = 100;
theat = rand(num,1)*2*pi;
r = rand(num,1);
s = r*300;
c = rand(num,3);
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic3.zhimg.com/80/v2-d85500232a4caf4a779ae1494c09d742_720w.jpg)

同时，**点的颜色也可以随输入参数变化**，再多表现一个维度。

颜色为RGB控制，在画图时，MATLAB会自动对其归一化。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
num = 100;
theat = rand(num,1)*2*pi;
r = rand(num,1);
s = r*300;
c = s;
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic4.zhimg.com/80/v2-f747459b0cda694d18d88a283b403abf_720w.jpg)

也可以将颜色与高度方向关联。

## 4 其他示例

**4.1 爱心**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
theat = 0:0.1:2*pi;
theat = theat';
num = length(theat);
r = 1 - sin(theat);
s = r*300;
c = [s,zeros(num,2)];
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic3.zhimg.com/80/v2-fdf38cb99596b3212d4167d5f6c1a882_720w.jpg)

**4.2 三叶草**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
theat = 0:0.02:2*pi;
r = cos(3*theat);
s = abs(r*300);
c = abs(s);
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic3.zhimg.com/80/v2-157dfb85efdff8e740ac6f98bcce96d6_720w.jpg)

**4.3 butterfly**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
theat = 0:0.05:2*pi;
theat = theat';
num = length(theat);
r = abs(sqrt(sin(theat).^5 + cos(theat).^5));
s = abs(r*300);
c = [s,rand(length(theat),2)];
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic3.zhimg.com/80/v2-195f49bd7f4feeb57cf749420fe3e96e_720w.jpg)

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
theat = 0:0.05:2*pi;
theat = theat';
num = length(theat);
r = abs(sqrt(sin(theat).^5 + cos(theat).^5));
s = abs(r*300);
c = [rand(length(theat),2),s];
p = polarscatter(theat,r,s,c,'filled','MarkerFaceAlpha',0.7); %设置透明度为0.7
```

![img](https://pic1.zhimg.com/80/v2-dbbb38a495dfbd84f5bb98121e3ac7e4_720w.jpg)