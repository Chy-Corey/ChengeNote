## 直方图

本文将从直方图开始，讲述histogram和histogram2函数的用法和示例。morebins、fewerbins、histcounts、histcounts2这几个函数并没有画图功能，而是histogram函数的辅助，用于设定或者求解某些值，也将在本文一并讲解。

**注意、注意、注意**，histogram用的非常多，**具备数据分组和画图功能**，非常强大。科研非常常见，用法也比较复杂，此文内容较为重要。

另外，需要区别**histogram函数**和**bar函数**。histogram函数是直方图，注重于统计，而bar是条形图，在图形方面设置更多。某些情况，两者可以一致。

### histogram函数

```matlab
histogram(X)
histogram(X,nbins)
histogram(X,edges)
histogram('BinEdges',edges,'BinCounts',counts)
histogram(C)
histogram(C,Categories)
histogram('Categories',Categories,'BinCounts',counts)
histogram(___,Name,Value)
histogram(ax,___)
h = histogram(___)
```

> histogram(X) 基于 X 创建直方图。histogram 函数使用自动 bin 划分算法，然后返回均匀宽度的 bin，这些 bin 可涵盖 X 中的元素范围并显示分布的基本形状。histogram 将 bin 显示为矩形，这样每个矩形的高度就表示 bin 中的元素数量。例如，histogram(X,nbins) 使用标量 nbins 指定的 bin 数量。histogram(X,edges) 将 X 划分到由向量 edges 来指定 bin 边界的 bin 内。每个 bin 都包含左边界，但不包含右边界，除了同时包含两个边界的最后一个 bin 外。
> histogram('BinEdges',edges,'BinCounts',counts) 手动指定 bin 边界和关联的 bin 计数。histogram 绘制指定的 bin 计数，而不执行任何数据的 bin 划分。例如，histogram(C)（其中 C 为分类数组）通过为 C 中的每个类别绘制一个条形来绘制直方图。
> histogram(C,Categories) 仅绘制 Categories 指定的类别的子集。
> histogram('Categories',Categories,'BinCounts',counts) 手动指定类别和关联的 bin 计数。histogram 绘制指定的 bin 计数，而不执行任何数据的 bin 划分。例如，histogram(___,Name,Value) 使用前面的任何语法指定具有一个或多个 Name,Value 对组参数的其他选项。例如，可以指定 'BinWidth' 和一个标量以调整 bin 的宽度，或指定 'Normalization' 和一个有效选项（'count'、 'probability'、 'countdensity'、 'pdf'、 'cumcount' 或 'cdf' ）以使用不同类型的归一化。有关属性列表，请参阅 Histogram 属性。
> histogram(ax,___) 将图形绘制到 ax 指定的坐标区中，而不是当前坐标区 (gca) 中。选项 ax 可以位于前面的语法中的任何输入参数组合之前。例如，h = histogram(___) 返回 Histogram 对象。使用此语法可检查并调整直方图的属性。有关属性列表，请参阅 Histogram 属性。

示例：

```matlab
a = randn(10000,1);
subplot(1,2,1)
h1 = histogram(a);
subplot(1,2,2)
h2 = histogram(a,20);
```

可以使用`NumBins`属性获取有多少个bin

#### 计数功能和频率

**请注意，这一小节非常、非常有用！！！**

histogram函数可以通过属性，查询每个条的边界坐标、个数和宽度，这样我们可以计算频率。Ps: histogram可以自行绘制PDF，但这里我们手动计算和绘制。

边界坐标、数量和宽度分别可以通过下面属性进行

```matlab
BinCounts
BinEdges
BinWidth
```

求频率分布如下

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1);

figure(1)
subplot(1,2,1)
h1 = histogram(a);
L = length(h1.BinEdges);
for i = 1:L-1
    x1(i) = (h1.BinEdges(i) + h1.BinEdges(i+1))/2;
end
y1 = h1.Values/10000/h1.BinWidth;
figure(2)
plot(x1,y1,'s','linewidth',2,'MarkerSize',10)

figure(1)
subplot(1,2,2)
h2 = histogram(a,20);
L = length(h2.BinEdges);
for i = 1:L-1
    x2(i) = (h2.BinEdges(i) + h2.BinEdges(i+1))/2;
end
y2 = h2.Values/10000/h2.BinWidth;
figure(2)
hold on
plot(x2,y2,'o','linewidth',2,'MarkerSize',10)
xlim([-4,4])
```

首先，通过

```matlab
(h1.BinEdges(i)+ h1.BinEdges(i+1))/2
```

求每个条中点的坐标。由于每个小条的面积就是频率，可通过

```matlab
h1.Values/h1.BinWidth/10000
```

求每个条的高度。频率图如下

![img](https://pic1.zhimg.com/80/v2-ae54d056a6a40cc068b7c7b71346260c_720w.jpg)

在1.2节中，不同分类条的高度是不同的，在这里，是一致的。

当然，上面是原理，下面利用MATLAB的属性来绘制。

MATLAB中可以直接将图的属性设置为'PDF'，即频率分布，如

```matlab
histogram(x,'Normalization','pdf')
```

下面是示例。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1);
h1 = histogram(a,21,'Normalization','pdf');
hold on
x = -4:0.1:4;
mu = 0;
sigma = 1;
y = exp(-(x-mu).^2./(2*sigma^2))./(sigma*sqrt(2*pi));
plot(x,y,'LineWidth',1.5)
figure(2)
h2 = histogram(a,21);
L = length(h2.BinEdges);
for i = 1:L-1
    x2(i) = (h2.BinEdges(i) + h2.BinEdges(i+1))/2;
end
y2 = h2.Values/10000/h2.BinWidth;
figure(1)
hold on
plot(x2,y2,'o','linewidth',1.5,'MarkerSize',8)
xlim([-4,4])
```

![img](https://pic1.zhimg.com/80/v2-d96065d4431d4cd0eaf4fed6d09156fc_720w.jpg)

点是我们自己计算的，可以发现计算结果与MATLAB一致，另外，标准正态分布概率密度函数为

![[公式]](https://www.zhihu.com/equation?tex=f%28x%2C%CE%BC%2C%CF%83%29%3D%5Cfrac%7B1%7D%7B%5Csigma%5Csqrt%7B2%5Cpi%7D%7D%7B%5Crm+exp%7D%5Cleft%28+-%5Cfrac%7B%5Cleft%28+x-%5Cmu+%5Cright%29%5E2%7D%7B2%5Csigma%5E2%7D+%5Cright%29)

令 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmu%3D0%EF%BC%8C%5Csigma%3D1) 带入，画在图中，可以看出频率与概率吻合较好。

除了pdf（频率），还有

- **'count'**：计数，默认格式, ![[公式]](https://www.zhihu.com/equation?tex=v_i%3Dc_i) ，为观测值的计数或频率；
- **'countdensity'**： ![[公式]](https://www.zhihu.com/equation?tex=v_i%3D%5Cfrac%7Bc_i%7D%7Bw_i%7D) ，按条的宽度进行换算的计数或频率。
- **'cumcount'**： ![[公式]](https://www.zhihu.com/equation?tex=v_i%3D%5Csum_%7Bj%3D1%7D%5E%7Bi%7D%7Bc_j%7D) ，累积计数。每个条的值等于该条及以前的所有条中的累积观测值数量。
- **'probability'**： ![[公式]](https://www.zhihu.com/equation?tex=v_i%3D%5Cfrac%7Bc_i%7D%7BN%7D) ，相对概率。
- **'pdf'**： ![[公式]](https://www.zhihu.com/equation?tex=v_i%3D%5Cfrac%7Bc_i%7D%7BNw_i%7D) ，概率密度函数的估计值。
- **'cdf'**： ![[公式]](https://www.zhihu.com/equation?tex=v_i%3D%5Csum_%7Bj%3D1%7D%5E%7Bi%7D%7B%5Cfrac%7Bc_j%7D%7BN%7D%7D) ，累积的密度函数估算值。



Normalization属性值：

| 值                  | bin 值                       | 注释                                                         |
| :------------------ | :--------------------------- | :----------------------------------------------------------- |
| `'count'`（默认值） | *v**i*=*c**i*                | 观测值的计数或频率。bin 值之和小于或等于 `numel(X)`。仅当某些输入数据不在 bin 中时，bin 值之和才会小于 `numel(X)`。对于分类数据，bin 值之和小于或等于 `numel(X)` 或 `sum(ismember(X(:),Categories))`。 |
| `'countdensity'`    | *v**i*=*c**i**w**i*          | 按 bin 的宽度进行换算的计数或频率。每个条形的面积（高 * 宽）是观测值在 bin 中的数量。条形面积之和小于或等于 `numel(X)`。对于分类直方图，这与 `'count'` 相同。**注意**`'countdensity'` 不支持日期时间和持续时间数据。 |
| `'cumcount'`        | *v**i*=*i**j*=1*c**j*       | 累积计数。每个 bin 的值等于该 bin 及以前的所有 bin 中的累积观测值数量。最后一个条形的高度小于或等于 `numel(X)`。对于分类直方图，最后一个条形的高度小于或等于 `numel(X)` 或 `sum(ismember(X(:),Categories))`。 |
| `'probability'`     | *v**i*=*c**i**N*             | 相对概率。条形高度之和小于或等于 `1`。                       |
| `'pdf'`             | *v**i*=*c**i**N*  ⋅   *w**i* | 概率密度函数的估计值。每个条形的面积是相对的观测值数量。条形面积之和小于或等于 `1`。对于分类直方图，这与 `'probability'` 相同。**注意**`'pdf'` 不支持日期时间和持续时间数据。 |
| `'cdf'`             | *v**i*=*i**j*=1 *c**j**N*   | 累积的密度函数估算值。每个条形的高度等于该 bin 及以前的所有 bin 中累积的相对观测值数量。最后一个条形的高度小于或等于 `1`。对于分类数据，每个条形的高度等于每个类别和所有先前类别中观测值的累积相对数量。 |

#### **指定边界**

在上面画图时，边界是默认的。histogram函数一般查找一组数据最大值和最小值，然后再分组。然而，当数据有一数据非常偏离时（实验中非常常见），会影响可视化效果，例如我在数据中添加了一个数据值15，图形如下所示。

原本是分类为20组，但是这个数据影响了分类，比较直观结果只有7组，其他的为0。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1);
a(10001,1) = 15;
h1 = histogram(a,20);
```

![img](https://pic2.zhimg.com/80/v2-dfa6ecb5f5ac0b427735136ece8eefbd_720w.jpg)

但histogram函数中，我们可以指定边界，并指定宽度，例如这里边界为-20到-4以及4到20，分别只产生一个条，-4到4每隔0.4产生一个条，即一共生成1+20+1=22个条。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1);
a(10001,1) = 15;
edges = [-20 -4:0.4:4 20];
subplot(1,2,1)
h1 = histogram(a,edges);
subplot(1,2,2)
h2 = histogram(a,edges);
xlim([-4,4])
```

![img](https://pic1.zhimg.com/80/v2-e69e1bf3e7561bcf4f5b26422e09eb14_720w.jpg)

这样可以将偏离值抽象掉，如左图所示。并通过调整x轴范围，得到可视化效果较好的图，如右图所示。

**此方法处理实验数据时，非常常见，有的人可能通过删除点的方式来实现，而我不建议这么做。**

另外，建议**当数据是对称时**，**条的个数最好设置为奇数**。

#### **其他属性**

其他属性主要是设定每个条的边界颜色和面颜色。示例如下

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1);
subplot(1,2,1)
h1 = histogram(a,21);
xlim([-4,4])
subplot(1,2,2)
h2 = histogram(a,21);
h2.FaceColor = [0.1 0.1 0.5];
h2.EdgeColor = 'r';
xlim([-4,4])
```

这里通过FaceColor更改面颜色，通过EdgeColor更改边颜色。

![img](https://pic4.zhimg.com/80/v2-d90e1041175446b71602a9498b370d9b_720w.jpg)

#### 绘制多个直方图

MATLAB可以将多个直方图绘制在一起，便于进行对比，例如

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(10000,1)-2;
h1 = histogram(a,21);
b = randn(10000,1);
hold on
h2 = histogram(b,21);
c = randn(10000,1)+2;
hold on
h3 = histogram(c,21);
xlim([-6,6])
```

![img](https://pic1.zhimg.com/80/v2-432fa70911f90552d84a110ae36b92fc_720w.jpg)

当然，histogram还有一些其他小功能，这里不赘述了，可以在属性中进行查找，或者参阅帮助文档[[1\]](https://zhuanlan.zhihu.com/p/345333630#ref_1)。

### histogram2函数

- `histogram2(X,Y)` 创建 `X` 和 `Y` 的二元直方图。`histogram2` 函数使用自动分 bin 算法，然后返回均匀面积的 bin，这些 bin 可涵盖 `X` 和 `Y` 中的元素范围并显示分布的基本形状。`histogram2` 将 bin 显示为三维矩形条形，这样每个条形的高度就表示 bin 中的元素数量。

- ` histogram2(X,Y,nbins)` 指定要在直方图的每个维度中使用的 bin 数量。

- `histogram2(X,Y,Xedges,Yedges)` 使用向量 `Xedges` 和 `Yedges` 指定每个维度中 bin 的边界。

histogram2函数即二维直方图。用法和histogram函数基本一致，但略有区别。

```matlab
a = randn(50000,1);
b = randn(50000,1);
subplot(2,2,1)
h1 = histogram2(a,b);
xlim([-4,4])
ylim([-4,4])
subplot(2,2,2)
h2 = histogram2(a,b,20);
xlim([-4,4])
ylim([-4,4])
subplot(2,2,3)
h3 = histogram2(a,b);
axis([-4,4,-4,4])
subplot(2,2,4)
h4 = histogram2(a,b,20);
axis([-4,4,-4,4])
```

![img](https://pic2.zhimg.com/80/v2-bcdc367683ef328c380da37ddde07b51_720w.jpg)

左边是默认分组，并有两个视角；右边是x和y方向都分成20组，并有两个视角，和histogram函数基本一致。

采用句柄 h.NumBins可查询分组数目，采用h.Values可以看分组情况。

如果要x和y分组不一样，采用

```matlab
histogram2(a,b,[m,n]);
```

例如

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = randn(50000,1);
b = randn(50000,1);
subplot(2,2,1)
h1 = histogram2(a,b,[15,5]);
xlim([-4,4])
ylim([-4,4])
subplot(2,2,2)
h2 = histogram2(a,b,[5,15]);
xlim([-4,4])
ylim([-4,4])
subplot(2,2,3)
h3 = histogram2(a,b,[15,5]);
axis([-4,4,-4,4])
subplot(2,2,4)
h4 = histogram2(a,b,[5,15]);
axis([-4,4,-4,4])
```

![img](https://pic3.zhimg.com/80/v2-8a9cbe5d79950fb46f23efdef00b65b2_720w.jpg)

左图将x分成15组，y分成5组；右图将x分成5组，y分成15组。

#### 对高度进行着色

```matlab
clear all
close all
a = randn(50000,1);
b = randn(50000,1);
subplot(2,2,1)
h1 = histogram2(a,b,[10,10],'FaceColor','flat');
xlim([-4,4])
ylim([-4,4])
subplot(2,2,2)
h2 = histogram2(a,b,[25,25],'FaceColor','flat');
xlim([-4,4])
ylim([-4,4])
subplot(2,2,3)
h3 = histogram2(a,b,[10,10],'FaceColor','flat');
axis([-4,4,-4,4])
subplot(2,2,4)
h4 = histogram2(a,b,[25,25],'FaceColor','flat');
axis([-4,4,-4,4])
```

![img](https://pic3.zhimg.com/80/v2-2909ad245b7cc53588d6313ae235db3a_720w.jpg)

#### 直接修改属性

```matlab
h = 
  Histogram2 with properties:

             Data: [10000x2 double]
           Values: [25x28 double]
          NumBins: [25 28]
        XBinEdges: [1x26 double]
        YBinEdges: [1x29 double]
         BinWidth: [0.3000 0.3000]
    Normalization: 'count'
        FaceColor: 'auto'
        EdgeColor: [0.1500 0.1500 0.1500]

  Show all properties
```

可以直接修改以上属性来更改直方图



```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
x = randn(1e6,1);
y = 3*x + randn(1e6,1);
b1 = histogram2(x,y,120,'FaceColor','flat');
b1.ShowEmptyBins = 'on';
set(gca,'ColorScale','log')
axis([-5,5,-15,15])
grid off
```

![img](https://pic4.zhimg.com/80/v2-371bf28a90ff7543b58a9cb130a6d53b_720w.jpg)

























