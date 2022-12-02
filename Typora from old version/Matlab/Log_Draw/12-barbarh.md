## 条形图

从本文开始，讲述MATLAB的**离散数据图**绘图函数。在MATLAB中，离散数据图一共包括四种类型，分别是：

- **条形图**，包括：bar 条形图、barh 水平条形图、bar3 绘制三维条形图、bar3h 绘制水平三维条形图和pareto 帕累托图，bar和bar3函数在[（一）](https://zhuanlan.zhihu.com/p/312069817)中有涉及，但没有详细讲述过；
- **针状图**，包括：stem和stem3函数，用于绘制离散序列数据，在[（一）](https://zhuanlan.zhihu.com/p/312069817)中有涉及，没有详细讲述过；
- **散点图**：包括：scatter和scatter3函数，用于绘制散点，详见[（二）](https://zhuanlan.zhihu.com/p/335615201)；
- **阶梯图**，包括：stairs函数，在[（一）](https://zhuanlan.zhihu.com/p/312069817)中有涉及，但没有详细讲述。

本文主要讲述条形图绘制，即bar、barh、bar3和bar3h四个函数。

**注意**：**条形图和直方图有本质区别**。直方图histogram函数非常强大，具备数据分组功能和绘图功能，详见[（六）](https://zhuanlan.zhihu.com/p/345333630)，而条形图只具备绘图功能。**直方图着重于数据分布，而条形图着重于离散数据展示**。

## 1 二维条形图bar和barh函数

**1.1 用法**

```matlab
bar(y)
bar(x,y)
bar(___,width)
bar(___,style)
bar(___,color)
bar(___,Name,Value)
bar(ax,___)
b = bar(___)
```

> bar(y) 创建一个条形图，y 中的每个元素对应一个条形。如果 y 是 m×n 矩阵，则 bar 创建每组包含 n 个条形的 m 个组。
> bar(x,y) 在 x 指定的位置绘制条形。
> bar(___,width) 设置条形的相对宽度以控制组中各个条形的间隔。将 width 指定为标量值。可以将此选项与前面语法中的任何输入参数组合一起使用。
> bar(___,style) 指定条形组的样式。例如，使用 'stacked' 将每个组显示为一个多种颜色的条形。
> bar(___,color) 设置所有条形的颜色。例如，使用 'r' 表示红色条形。
> bar(___,Name,Value) 使用一个或多个名称-值对组参数指定条形图的属性。仅使用默认 'grouped' 或 'stacked' 样式的条形图支持设置条形属性。在所有其他输入参数之后指定名称-值对组参数。有关属性列表，请参阅 Bar 属性。
> bar(ax,___) 将图形绘制到 ax 指定的坐标区中，而不是当前坐标区 (gca) 中。选项 ax 可以位于前面的语法中的任何输入参数组合之前。
> b = bar(___) 返回一个或多个 Bar 对象。如果 y 是向量，则 bar 将创建一个 Bar 对象。如果 y 是矩阵，则 bar 为每个序列返回一个 Bar 对象。显示条形图后，使用 b 设置条形的属性。[[1\]](https://zhuanlan.zhihu.com/p/346070927#ref_1)

**注意**，bar和barh函数绘图原理、用法等完全一致。

**1.2 示例1**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:8;
y = rand(length(x),1);

subplot(3,1,1)
bar(x,y)

subplot(3,1,2)
bar(x,y,1)

subplot(3,1,3)
bar(x,y,0.5)
```

![img](https://pic3.zhimg.com/80/v2-fa2935c8abd4096aff0ebf44250b5d6e_720w.jpg)

**可以更改条形图的间隔**。图1位默认宽度，阈值为0.8，这里可以改成1和0.5，分别如图2和3所示。注意，阈值为1的图和直方图**效果一致**。

默认为垂直分布，通过barh函数可绘制水平条形图，如下图所示。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:8;
y = rand(length(x),1);

subplot(1,3,1)
barh(x,y)

subplot(1,3,2)
barh(x,y,1)

subplot(1,3,3)
barh(x,y,0.5)
```

![img](https://pic3.zhimg.com/80/v2-ba0faef663585a3e6bac71d94e524426_720w.jpg)

**1.3 示例2**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:4;
y = rand(length(x),4);

subplot(3,1,1)
bar(x,y)

subplot(3,1,2)
bar(x,y,'stacked')

x = categorical({'just','pure','alien'}); %自动排序
x = reordercats(x,{'just','pure','alien'}); %强制恢复之前排序
subplot(3,1,3)
bar(x,y,1)
```

![img](https://pic1.zhimg.com/80/v2-3d63c10590fa13988508814407f044c4_720w.jpg)

可以**绘制多组数据的条形图**，如图1所示，在默认情况下，绘制方式为分组，可以在函数中添加 'stacked' 命令画**堆叠图，**如图2所示。

可以为**每一类数据添加名称**，注意，注意，注意，**bar函数会自动对名称进行排序**，若要恢复原有顺序，需要用 reordercats 函数，详见代码。

水平条形图效果如下所示。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:4;
y = rand(length(x),4);

subplot(1,3,1)
barh(x,y)

subplot(1,3,2)
barh(x,y,'stacked')

x = categorical({'just','pure','alien'}); %自动排序
x = reordercats(x,{'just','pure','alien'}); %强制恢复之前排序
subplot(1,3,3)
barh(x,y,1)
```

![img](https://pic2.zhimg.com/80/v2-5f3bbb003cba79d645c78b333ca35a29_720w.jpg)

**1.4 示例3**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:4;
y = rand(length(x),3);
subplot(2,1,1)
b = bar(x,y);
b(1).LineWidth = 1;
b(2).LineWidth = 1;
b(3).LineWidth = 1;

subplot(2,1,2)
b = bar(x,y);
b(1).FaceColor = [0.1 0.5 0.9];
b(2).FaceColor = [0.9 0.1 0.5];
b(3).FaceColor = [0.5 0.9 0.1];
b(1).EdgeColor = [0.1 0.5 0.9];
b(2).EdgeColor = [0.9 0.1 0.5];
b(3).EdgeColor = [0.5 0.9 0.1];
```

![img](https://pic1.zhimg.com/80/v2-9b410c86ab06beb42aeab61cce3feca0_720w.jpg)

可以通过返回**句柄对图形属性进行修改**，包括线属性和面属性，可以更改颜色、线宽等参数。

水平条形图效果如下所示。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = 2:4;
y = rand(length(x),3);
subplot(1,2,1)
b = barh(x,y);
b(1).LineWidth = 1;
b(2).LineWidth = 1;
b(3).LineWidth = 1;

subplot(1,2,2)
b = barh(x,y);
b(1).FaceColor = [0.1 0.5 0.9];
b(2).FaceColor = [0.9 0.1 0.5];
b(3).FaceColor = [0.5 0.9 0.1];
b(1).EdgeColor = [0.1 0.5 0.9];
b(2).EdgeColor = [0.9 0.1 0.5];
b(3).EdgeColor = [0.5 0.9 0.1];
```

![img](https://pic2.zhimg.com/80/v2-104dbda9ab029969e2e28e368a8a7b31_720w.jpg)

## 2 三维条形图bar3和bar3h函数

**2.1 用法**

```matlab
bar3(Z)
bar3(Y,Z)
bar3(...,width)
bar3(...,style)
bar3(...,color)
bar3(ax,...)
h = bar3(...)
```

> bar3 绘制三维条形图。
> bar3(Z) 绘制三维条形图，Z 中的每个元素对应一个条形图。如果 Z 是向量，y 轴的刻度范围是从 1 至 length(Z)。如果 Z 是矩阵，则 y 轴的刻度范围是从 1 到 Z 的行数。
> bar3(Y,Z) 在 Y 指定的位置绘制 Z 中各元素的条形图，其中 Y 是为垂直条形定义 y 值的向量。y 值可以是非单调的，但不能包含重复值。如果 Z 是矩阵，则 Z 中位于同一行内的元素将出现在 y 轴上的相同位置。
> bar3(...,width) 设置条形宽度并控制组中各个条形的间隔。默认 width 为 0.8，条形之间有细小间隔。如果 width 为 1，组内的条形将紧挨在一起。
> bar3(...,style) 指定条形的样式。style 是 'detached'、'grouped' 或 'stacked'。显示的默认模式为 'detached'。

- 'detached' 在 x 方向上将 Z 中的每一行的元素显示为一个接一个的单独的块。
- 'grouped' 显示 n 组的 m 个垂直条，其中 n 是行数，m 是 Z 中的列数。每组包含一个对应于 Z 中每列的条形。
- 'stacked' 为 Z 中的每行显示一个条形。条形高度是行中元素的总和。每个条形标记有多种颜色，不同颜色分别对应不同的元素，显示每行元素占总和的相对量。

> bar3(...,color) 使用 color 指定的颜色显示所有条形。例如，使用 'r' 表示红色条形。可将 color 指定为下列值之一：'r'、'g'、'b'、'c'、'm'、'y'、'k' 或 'w'。
> bar3(ax,...) 将图形绘制到 ax 坐标区中，而不是当前坐标区 (gca) 中。
> h = bar3(...) 返回由 Surface 对象组成的向量。如果 Z 是矩阵，则 bar3 将为 Z 中的每一列创建一个 Surface 对象。[[2\]](https://zhuanlan.zhihu.com/p/346070927#ref_2)

bar3函数与bar函数原理、用法基本一致。

**2.2 示例1**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
m = 5; n = 6;
for i = 1:m
    for j = 1:n
        a(i,j) = rand(1) + (j-1)*0.7;
    end
end
bar3(a)
```

![img](https://pic2.zhimg.com/80/v2-9dc8ec485941210d53f97655e0317c21_720w.jpg)

这里随机产生数据，注意，bar3函数默认是 'detached' 分组模式，间隔是0.8，这里可以对间隔和分组模式等进行更改。

间隔与bar函数一样，阈值在[0 1]之间。分组模式有三种，分别是

- detached'
- 'grouped'
- 'stacked'

可以根据需要进行设置。例如，下图将间隔设置为1，分组设置为'stacked' 。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
m = 5; n = 4;
for i = 1:m
    for j = 1:n
        a(i,j) = rand(1) + (j-1)*0.2;
    end
end
bar3(a,1,'stacked')
```

![img](https://pic2.zhimg.com/80/v2-93deb0bde126ac8b3ba10d74a8202de1_720w.jpg)

**2.3 示例2**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
m = 5; n = 3;
for i = 1:m
    for j = 1:n
        a(i,j) = rand(1) + (j-1)*0.7;
    end
end
b = bar3(a);
b(1).FaceColor = [0.1 0.5 0.9];
b(2).FaceColor = [0.9 0.1 0.5];
b(3).FaceColor = [0.5 0.9 0.1];
```

![img](https://pic3.zhimg.com/80/v2-25f161fa9e000461fbcb1a85fedb9baa_720w.jpg)

同样的，可以返回句柄对图形属性进行自定义设置，这里对颜色进行自定义修改，如上图所示。

**2.4 示例3**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
m = 5; n = 6;
for i = 1:m
    for j = 1:n
        a(i,j) = rand(1) + (j-1)*0.7;
    end
end
bar3h(a)
```

![img](https://pic2.zhimg.com/80/v2-8db79530b39c3e75d055294264c77f9d_720w.jpg)

bar3h函数和bar3函数用法完全一致，如上图所示。

**2.5 示例4**

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
m = 5; n = 3;
for i = 1:m
    for j = 1:n
        a(i,j) = rand(1) + (j-1)*0.7;
    end
end
b = bar3h(a);
b(1).FaceColor = [0.1 0.5 0.9];
b(2).FaceColor = [0.9 0.1 0.5];
b(3).FaceColor = [0.5 0.9 0.1];
```

![img](https://pic2.zhimg.com/80/v2-4b003c939498edf042f3942f21543a0d_720w.jpg)

可以返回句柄对图形属性进行自定义设置，这里对颜色进行自定义修改。