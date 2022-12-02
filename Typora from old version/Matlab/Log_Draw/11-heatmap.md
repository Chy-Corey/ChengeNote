## 热图heatmap函数

热图，是统计分布图的一种，科研中时而见到。笔者经常在大牛们的论文中看到，尤其是计算机领域。

在MATALB中，可采用heatmap函数绘制热图，并结合sortx和sorty两个函数对其进一步操作。

注意，从R2017a版本开始有heatmap函数，从R2017b版本开始有sortx和sorty函数。

#### 用法

```matlab
heatmap(tbl,xvar,yvar)
heatmap(tbl,xvar,yvar,'ColorVariable',cvar)
heatmap(cdata)
heatmap(xvalues,yvalues,cdata)
heatmap(___,Name,Value)
heatmap(parent,___)
h = heatmap(___)
```

> heatmap(tbl,xvar,yvar) 基于表 tbl 创建一个热图。xvar 输入参数指示沿 x 轴显示的表变量。yvar 输入参数指示沿 y 轴显示的表变量。默认颜色基于计数聚合，这种方法计算每对 x 和 y 值一起出现在表中的总次数。
> heatmap(tbl,xvar,yvar,'ColorVariable',cvar) 使用 cvar 指定的表变量来计算颜色数据。默认的计算方法为均值聚合。
> heatmap(cdata) 基于矩阵 cdata 创建一个热图。热图上的每个单元格对应 cdata 中的一个值。
> heatmap(xvalues,yvalues,cdata) 指定沿 x 轴和 y 轴显示的值的标签。
> heatmap(___,Name,Value) 使用一个或多个名称-值对组参数指定热图的其他选项。请在所有其他输入参数之后指定这些选项。有关属性列表，请参阅 HeatmapChart 属性。
> heatmap(parent,___) 在由 parent 指定的图窗、面板或选项卡上创建热图。
> h = heatmap(___) 返回 HeatmapChart 对象。创建图后，使用 h 修改图属性。有关属性列表，请参阅 HeatmapChart 属性。[[1\]](https://zhuanlan.zhihu.com/p/345926902#ref_1)

heatmap是**具有数据分组功能**的，可以**自动对数据进行分组**，和histogram函数很像，但需要用Table数据类型。

#### 示例1

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = rand(5,5);
h = heatmap(a);
```

![img](https://pic2.zhimg.com/80/v2-f7b1bccccb86b9a28949167ce50e1bf9_720w.jpg)

上图是一个典型的热图，信息包括：

- x维度
- y维度
- x和y维度对应的值
- 方格颜色
- 值的标注
- 图例

可以通过句柄，对上面参数进行自定义设置。

#### 示例2

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = rand(5,5);
h = heatmap(a);
h.CellLabelFormat = '%0.2f';
```

![img](https://pic4.zhimg.com/80/v2-4b3b553b1786c15eb3bef74e6e692317_720w.jpg)

可以通过h.CellLabelFormat句柄，对标注值的小数位数进行设置，如 '%0.2f' 表示保留两个小数。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = rand(5,5);
h = heatmap(a);
h.CellLabelFormat = '%0.2f';
h.CellLabelColor = 'none';
```

![img](https://pic1.zhimg.com/80/v2-7fe9689af4e5f8e9814559c6fa858684_720w.jpg)

进一步的，可以对标注值的颜色进行修改，设为 'none' 表明去掉标注值。

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = rand(5,5);
h = heatmap(a);
h.CellLabelFormat = '%0.2f';
% h.CellLabelColor = 'none';
colormap(gca, 'parula')
```

![img](https://pic2.zhimg.com/80/v2-4b1b2f7aa985a0897efa500cb4c7a5a5_720w.jpg)

可以通过colormap命宁，对图例颜色进行更改，例如设置为 'parula'。

在实际中，笔者印象好像是默认用的最多。

#### 示例3

```matlab
clc %https://zhuanlan.zhihu.com/p/312069817
clear all
close all
a = rand(6,6);
xname = {'i','am','just','a','pure','alien'};
yname = {'I','AM','JUST','A','PURE','ALIEN'};
h = heatmap(xname,yname,a);
h.CellLabelFormat = '%0.2f';
% h.CellLabelColor = 'none';
% colormap(gca, 'parula')
```

![img](https://pic1.zhimg.com/80/v2-f359eec51cdce1e3999b31b133e1f364_720w.jpg)

也能对x和y轴进行定义。

例如，笔者的签名是 'I am just a pure alien'，可以为其设置。注意，名称维度和矩阵数据维度要统一。