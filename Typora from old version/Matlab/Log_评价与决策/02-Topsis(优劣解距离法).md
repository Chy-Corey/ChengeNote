## Topsis(优劣解距离法)

### 简介

Technique forOrder Preference by Similarity to an Ideal(优劣解距离法)，该方法可以根据现有的数据，对个体进行评价排序。根据有限个评价对象与理想化目标的接近程度进行排序的方法，是在现有的对象中进行相对优劣的评价。

TOPSIS中，理想解和负理想解是TOPSIS中的两个基本概念。理想解是最优的解，它的各个属性值都达到各备选方案中的最好的值；负理想解是一设想的最劣的解，它的各个属性值都达到各备选方案中的最坏的值。

### 等作用情况（权重相同）

#### 衡量接近程度的方法

1. 用绝对值判别

2. 用欧氏距离判别：

   [推荐系统-- 欧式距离和相似度 - 简书 (jianshu.com)](https://www.jianshu.com/p/9cd5c1fbe9eb)

#### 指标的处理（正向化）

有的数据是越大越好，有的越小越好，有的越靠近某个值宇浩，有的在一个区间中越好，为了提高算法的泛用性，需要对数据进行正向化处理，都让它们越大越好。

1. 极大型指标：越大越好
2. 极小型指标：越小越好
3. 中间型指标：越靠近某个值越好
4. 区间型指标：在某个区间范围内最好，区间中数值无优劣之分

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_15-53-02.jpg" alt="Snipaste_2021-12-27_15-53-02" style="zoom: 67%;" />

#### 指标的处理（标准化）

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_15-55-03.jpg" alt="Snipaste_2021-12-27_15-55-03" style="zoom:67%;" />

#### 最优最劣方案集的确定

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_15-55-55.jpg" alt="Snipaste_2021-12-27_15-55-55" style="zoom:67%;" />

#### Example

```matlab
% Topsis算法基本思想：基于归一化后的原始数据矩阵，采用余弦法找出有限方案中的最优方案和最劣方案（分别用最优向量和最劣向量表示），
% 然后分别计算各评价对象与最优方案和最劣方案间的距离，获得各评价对象与最优方案的相对接近程度，以此作为评价优劣的依据。
% 案例代码：x是需要评价的对象矩阵
x=[
21584  76.7  7.3  1.01  78.3  97.5  2.0
24372  86.3  7.4  0.80  91.1  98.0  2.0
22041  81.8  7.3  0.62  91.1  97.3  3.2
21115  84.5  6.9  0.60  90.2  97.7  2.9
24633  90.3  6.9  0.25  95.5  97.9  3.6];%矩阵
[n,m]=size(x);
%将3，4，7的低优指标去倒数转化为高优指标并且把所有指标换成接近的大小
x(:,1)=x(:,1)/100;
x(:,3)=(1./x(:,3))*100;
x(:,4)=(1./x(:,4))*100;
x(:,7)=(1./x(:,7))*100;
zh=zeros(1,m);
d1=zeros(1,n); %最小值矩阵
d2=zeros(1,n); %最大值矩阵
c=zeros(1,n);  %接近程度
%归一化
for i=1:m
    for j=1:n
        zh(i)=zh(i)+x(j,i)^2;
    end
end
for i=1:m
    for j=1:n
       x(j,i)=x(j,i)/sqrt( zh(i));
    end
end
%计算距离
xx=min(x);
dd=max(x);
for i=1:n
    for j=1:m
        d1(i)=d1(i)+(x(i,j)-xx(j))^2;
    end
    d1(i)=sqrt(d1(i));
end
for i=1:n
    for j=1:m
        d2(i)=d2(i)+(x(i,j)-dd(j))^2;
    end
    d2(i)=sqrt(d2(i));
end
%计算接近程度
for i=1:n
    c(i)=d1(i)/(d2(i)+d1(i));
end

```

### 权重不同

上例的计算，默认评价因素之间重要程度相同，实际中并非如此，各个因素之间有重要的程度之别。如何给每个因素设置重要程度呢？

其实就是在计算欧氏距离时，在每一项之前×权重。

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_16-17-42.jpg" style="zoom:67%;" />

这里的权重不是认为给定的，是熵权法计算出来的。

## 熵权法

熵权法是一种客观赋权方法。

原理：指标的变异程度越小，所反应的信息量也就越少，其对应的权值也就越低。

信息熵的定义，请看ppt测量信息论。

#### 权重计算

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_16-39-38.jpg" style="zoom:67%;" />

<img src="E:\Storage\博客\Typora\Matlab\Source\img\Snipaste_2021-12-27_16-39-42.jpg" style="zoom:67%;" />

相关资料：[熵权法评价估计详细原理讲解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/267259810)

