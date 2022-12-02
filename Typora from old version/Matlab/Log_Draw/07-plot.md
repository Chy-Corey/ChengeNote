# MATLAB 绘图（Plot）

要绘制函数的图形，需要执行以下步骤：

- 通过指定变量 x 的值范围来定义 x，为此函数将绘制出来
- 定义函数， **y = f(x)**
- 调用 **plot** 命令，如下 **plot(x, y)**

下面的实例将演示这个概念。让我们绘制一个简单的函数y=x，x的取值范围为0到100，增量为5。

创建一个脚本文件并输入以下代码-

```
x = [0:5:100];
y = x;
plot(x, y)
```

运行文件时，MATLAB显示以下图-

![绘制y = x](https://www.cainiaojc.com/static/upload/210417/1303590.jpg)

让我们再举一个实例来绘制函数y = x 2。在此示例中，我们将绘制两个具有相同功能的图形，但是第二次，我们将减小增量值。请注意，随着我们减少增量，图形会变得更加平滑。

创建一个脚本文件并输入以下代码-

```
x = [1 2 3 4 5 6 7 8 9 10];
x = [-100:20:100];
y = x.^2;
plot(x, y)
```

运行文件时，MATLAB显示以下图-

![绘制y = x ^ 2](https://www.cainiaojc.com/static/upload/210417/1303591.jpg)

稍微更改代码文件，将增量减少到5-

```
x = [-100:5:100];
y = x.^2;
plot(x, y)
```

MATLAB绘制更平滑的图形-

![以较小的增量绘制y = x ^ 2](https://www.cainiaojc.com/static/upload/210417/1303592.jpg)

## 在图形上添加标题，标签，网格线和缩放

MATLAB 允许您添加标题、沿 x 轴和 y 轴的标签、网格线，并且还可以调整轴以使图形更漂亮。

- **xlabel** 和 **ylabel** 命令产生沿x轴和y轴的标签。
- **title** 命令允许您在图形上放置标题。
- **grid on** 命令允许您将网格线放在图形上。
- **axis equal** 命令允许使用相同的比例因子和两个轴上的间距生成图。
- **axis square** 命令生成一个正方形图。

## 实例

创建一个脚本文件并输入以下代码-

```matlab
x = [0:0.01:10];
y = sin(x);
plot(x, y), xlabel('x'), ylabel('Sin(x)'), title('Sin(x) Graph'),
grid on, axis equal
```

MATLAB生成以下图形-

![整理我们的图表](https://www.cainiaojc.com/static/upload/210417/1303593.jpg)

## 在同一图形上绘制多个函数

您可以在同一图上绘制多个图形。以下示例演示了概念-

## 实例

创建一个脚本文件并输入以下代码-

```
x = [0 : 0.01: 10];
y = sin(x);
g = cos(x);
plot(x, y, x, g, '.-'), legend('Sin(x)', 'Cos(x)')
```

MATLAB生成以下图形-

![同一图上的多个函数](https://www.cainiaojc.com/static/upload/210417/1303594.jpg)

## 在图形上设置颜色

MATLAB提供了八种基本的颜色选项来绘制图形。下表显示了颜色及其代码-

| 代码 |  颜色  |
| :--: | :----: |
|  w   |  白色  |
|  k   |  黑色  |
|  b   |  蓝色  |
|  r   |  红色  |
|  c   |  青色  |
|  g   |  绿色  |
|  m   | 洋红色 |
|  y   |  黄色  |

## 实例

让我们画出两个多项式的图

- f(x)= 3x 4 + 2x 3 + 7x 2 + 2x + 9和
- g(x)= 5x 3 + 9x + 2

创建一个脚本文件并输入以下代码-

```
x = [-10 : 0.01: 10];
y = 3*x.^4 + 2 * x.^3 + 7 * x.^2 + 2 * x + 9;
g = 5 * x.^3 + 9 * x + 2;
plot(x, y, 'r', x, g, 'g')
```

运行文件时，MATLAB生成以下图形-

![图形上的颜色](https://www.cainiaojc.com/static/upload/210417/1303595.jpg)

## 设定轴比例

**axis**命令允许您设置轴刻度。您可以按以下方式使用axis命令提供x和y轴的最小值和最大值：

```
axis ( [xmin xmax ymin ymax] )
```

以下示例显示了这一点-

## 实例

创建一个脚本文件并输入以下代码-

```
x = [0 : 0.01: 10];
y = exp(-x).* sin(2*x + 3);
plot(x, y), axis([0 10 -1 1])
```

运行文件时，MATLAB生成以下图形-

![设定轴比例](https://www.cainiaojc.com/static/upload/210417/1303596.jpg)

## 生成子图

在同一图形中创建一个绘图数组时，每个绘图都称为子绘图。**subplot** 命令用于创建子图。

该命令的语法是-

```
subplot(m, n, p)
```

其中，*m*和*n*是绘图数组的行数和列数，而*p*指定放置特定绘图的位置。

使用subplot命令创建的每个图都可以具有自己的特征。以下示例演示了概念-

## 实例

让我们生成两个图-

y = e −1.5x sin(10x)

y = e -2x sin(10x)

创建一个脚本文件并输入以下代码-

```
x = [0:0.01:5];
y = exp(-1.5*x).*sin(10*x);
subplot(1,2,1)
plot(x,y), xlabel('x'),ylabel('exp(–1.5x)*sin(10x)'),axis([0 5 -1 1])
y = exp(-2*x).*sin(10*x);
subplot(1,2,2)
plot(x,y),xlabel('x'),ylabel('exp(–2x)*sin(10x)'),axis([0 5 -1 1])
```

运行文件时，MATLAB生成以下图形-

![生成子图](https://www.cainiaojc.com/static/upload/210417/1303597.jpg)

线型、颜色、符号可选择如下



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

```matlab
x = 0:0.1:3;
y1 = x;
y2 = x.^2;
y3 = x.^3;
plot(x,y1,'red-',x,y2,'blueo',x,y3,'black-o')
title('示意图')
xlabel('x的变化')
ylabel('y的变化')
text(0.5,25,'说明示例')
legend('线性y=x','二次方y=x^2','三次方y=x^3')
```

在绘制图形时，Matlab可以自动根据要绘制曲线数据的范围选择合适的坐标刻度，使得曲线能够尽可能清晰的显示出来。所以，一般情况下用户不必选择坐标轴的刻度范围。但是，如果用户对坐标不满意，可以利用axis函数对其重新设定。

```matlab
axis equal  %纵横坐标轴采用等长刻度
axis square %产生正方形坐标系（默认为矩形）
axis auto   %使用默认设置
axis off    %取消坐标轴
axis on     %显示坐标轴
grid on     %显示网格
grid off    %不显示网格
box on      %显示其他轴(上、右)
box off     %不显示其他轴(上、右)
hold on     %继续在同一图层绘图
figure(1)   %新生成一个图层
```

## 指定线宽、标记大小和标记颜色

```
x = -pi:pi/10:pi;
y = tan(sin(x)) - sin(tan(x));

figure
plot(x,y,'--gs',...
    'LineWidth',2,...
    'MarkerSize',10,...
    'MarkerEdgeColor','b',...
    'MarkerFaceColor',[0.5,0.5,0.5])
```

![Figure contains an axes. The axes contains an object of type line.](https://ww2.mathworks.cn/help/matlab/ref/specifylinewidthmarkersizeandmarkercolorexample_01_zh_CN.png)

## 常见图形对象

当调用函数以便创建图形时，MATLAB 会创建图形对象的层次结构。例如，调用 [`plot`](https://ww2.mathworks.cn/help/matlab/ref/plot.html) 函数会创建下列图形对象：

- 图窗 - 包含轴、工具栏、菜单等的窗口。
- 轴 - 包含表示数据的对象的坐标系
- 线条 - 代表传递至 `plot` 函数的数据值的线条。
- 文本 - 用于轴刻度线和可选标题及注释的标签。

不同类型的图形使用不同对象来表示数据。由于存在许多种图形，因此也存在许多数据对象类型。其中一些用于一般用途，例如线条和矩形，还有一些是用于高度专业的用途，例如误差条、颜色栏和图例。

#### 访问对象属性

绘图函数可返回用于创建图形的对象。例如，以下语句将创建一个图形并返回由 `plot` 函数创建的线条对象：

```
x = 1:10;
y = x.^3;
h = plot(x,y);
```

使用 `h` 来设置线条对象的属性。例如，设置它的 `Color` 属性。

```
h.Color = 'red';
```

此外，也可以在调用绘图函数时指定线条属性。

```
h = plot(x,y,'Color','red');
```

可以查询线条属性以便查看当前值：

```
h.LineWidth
ans =
	0.5000
```

#### 查找对象的属性

要查看对象的属性，请输入：

```
get(h)
```

MATLAB 将返回包含对象属性及当前值的列表。

要查看对象属性及可能的值信息，请输入：

```
set(h)
```

### 设置对象属性

可使用 [`set`](https://ww2.mathworks.cn/help/matlab/ref/set.html) 函数一次设置多个属性。



#### 设置现有对象的属性

要对多个对象的同一属性设置相同值，请使用 `set` 函数。

例如，下面的语句绘制一个 5×5 矩阵（创建五个线条对象，每列各一个），然后将 `Marker` 属性设置为正方形，并将 `MarkerFaceColor` 属性设置为绿色。

```
y = magic(5);
h = plot(y);
set(h,'Marker','s','MarkerFaceColor','g')
```

在本示例中，`h` 是一个包含五个句柄的向量，图形中的每个线条（共五个）各一个句柄。`set` 语句将所有线条的 `Marker` 和 `MarkerFaceColor` 属性设置为相同值。

要对一个对象设置属性值，请对句柄数组建立索引：

```
h(1).LineWidth = 2;
```

#### 设置多个属性值

如果要将每个线条的属性设置为不同值，您可以使用元胞数组存储所有数据，并将其传递给 [`set`](https://ww2.mathworks.cn/help/matlab/ref/set.html) 命令。例如，创建绘图并保存线条句柄：

```
figure
y = magic(5);
h = plot(y);
```

假定您要为每个线条添加不同标记，并使标记的面颜色与线条的颜色相同。您需要定义两个元胞数组，一个包含属性名，另一个包含属性所需的值。

`prop_name` 元胞数组包含两个元素：

```
prop_name(1) = {'Marker'};
prop_name(2) = {'MarkerFaceColor'};
```

`prop_values` 元胞数组包含 10 个值：`Marker` 属性有 5 个值，`MarkerFaceColor` 属性有 5 个值。请注意，`prop_values` 是一个二维元胞数组。第一个维表示值应用于 `h` 中的哪个句柄，第二个维表示值分配给哪个属性：

```
prop_values(1,1) = {'s'};
prop_values(1,2) = {h(1).Color};
prop_values(2,1) = {'d'};
prop_values(2,2) = {h(2).Color};
prop_values(3,1) = {'o'};
prop_values(3,2) = {h(3).Color};
prop_values(4,1) = {'p'};
prop_values(4,2) = {h(4).Color};
prop_values(5,1) = {'h'};
prop_values(5,2) = {h(5).Color};
```

`MarkerFaceColor` 始终分配到相应线条的颜色的值（通过获取线条 `Color` 属性获得）。

定义元胞数组之后，调用 `set` 以便指定新属性值：

```
set(h,prop_name,prop_values)
```

![img](https://ww2.mathworks.cn/help/matlab/learn_matlab/magicsq_zh_CN.png)
