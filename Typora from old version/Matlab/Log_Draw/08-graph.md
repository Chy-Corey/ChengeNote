## Matlab图形



#### 线性直角坐标图

- 条形图 bar(x,y)
- 阶梯图 stairs(x,y)
- 杆图 stem(x,y)
- 填充图 fill(x,y)
- fill函数按向量元素下标渐增次序依次用直线段连接x，y对应元素定义的数据点。

示例

```matlab
% 
x = 0:0.35:7;
y = 2*exp(-0.5*x);
subplot(2,2,1);bar(x,y,'g');
title('bar(x,y,''g'')');axis([0, 7, 0 ,2]);
subplot(2,2,2);fill(x,y,'r');
title('fill(x,y,''r'')');axis([0, 7, 0 ,2]);
subplot(2,2,3);stairs(x,y,'b');
title('stairs(x,y,''b'')');axis([0, 7, 0 ,2]);
subplot(2,2,4);stem(x,y,'k');
title('stem(x,y,''k'')');axis([0, 7, 0 ,2]);
```

#### 极坐标图

polar函数用来绘制极坐标图

用法

```matlab
polar(theta,rho,'options')
```

theta为极坐标极角，rho为极径，选项的内容和plot函数相似。

示例

```matlab
theat = 0:0.1:4*pi;
y1 = sin(1/2*theat);
y2 = sin(theat).*cos(theat);
subplot(2,2,1)
plot(theat,y1,'ro')
subplot(2,2,2)
plot(theat,y2,'b-')
subplot(2,2,3)
polar(theat,y1,'ro')
grid on
subplot(2,2,4)
polar(theat,y2,'b-')
grid on
```

#### 对数坐标

- x轴线性，y轴线性

```matlab
plot(x,y)
```

- x轴对数，y轴线性

```matlab
semilogx(x,y)
```

- x轴线性，y轴对数

```matlab
semilogy(x,y)
```

- x轴对数，y轴对数

```matlab
loglog(x,y)
```

示例

```matlab
x = 1:1000;
y = x.^2+exp(x);
subplot(2,2,1)
plot(x,y,'r-')
subplot(2,2,2)
semilogx(x,y,'b-')
subplot(2,2,3)
semilogy(x,y,'m-')
subplot(2,2,4)
loglog(x,y,'k-')
```

#### **符号函数**

- 显函数

```matlab
ezplot('f(x)',[a,b])
```

- 隐函数

```matlab
ezplot('f(x,y)',[xmin,xmax,ymin,ymax])
```

- 参数方程

```matlab
ezplot('x(t)','y(t)',[tmin,tmax])
```

示例1

```matlab
subplot(3,1,1)
ezplot('sin(x)',[0,2*pi])
subplot(3,1,2)
ezplot('exp(x)+sin(x*y)',[-2,2,-1000,1000])
subplot(3,1,3)
ezplot('3*t','sin(t)*cos(t)',[0,2*pi])
```

#### 空间曲线

plot3函数

类似plot2，用法一致

用法：一条

```matlab
plot3(x,y,z,'options')
```

用法：多条

```matlab
plot3(x1,y1,z1,'options1',x2,y2,z2,'options2',...)
```

示例1

```matlab
x = 1:0.1:10;
y = 1:0.1:10;
z = x.^2+y;
plot3(x,y,z,'r-o')
grid on
```

![img](https://pic3.zhimg.com/80/v2-d4e94a9cdd9ef417ec888b08aca0d772_720w.jpg)

示例2：参数

```matlab
t = 0:pi/50:10*pi;
y1 = sin(t);
y2 = cos(t);
plot3(y1,y2,t,'ro');
grid on
```

![img](https://pic3.zhimg.com/80/v2-671912c8532c5d7a32d9fc4ca8c809fa_720w.jpg)

示例3

```matlab
x = -3:0.2:3;
y = 1:0.2:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,1)
plot3(X,Y,Z)
subplot(2,2,2)
plot3(X,Y,Z)
shading flat
 
x = -3:0.1:3;
y = 1:0.1:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,3)
plot3(X,Y,Z)
subplot(2,2,4)
plot3(X,Y,Z)
shading flat
```

![img](https://pic1.zhimg.com/80/v2-d28ebb4390585016eecbcfd30fc0cc58_720w.jpg)

#### 空间曲面

##### surf函数

用法

```matlab
surf(x,y,z)
```

示例

```matlab
x = -3:0.2:3;
y = 1:0.2:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,1)
surf(X,Y,Z)
subplot(2,2,2)
surf(X,Y,Z)
shading flat

x = -3:0.1:3;
y = 1:0.1:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,3)
surf(X,Y,Z)
subplot(2,2,4)
surf(X,Y,Z)
shading flat
```

![img](https://pic4.zhimg.com/80/v2-7a496e2141e85bccaa70f15995df30fb_720w.jpg)

**shading：设置颜色着色属性**

语法

```
shading flatshading facetedshading interpshading(axes_handle,...)
```

说明

`shading` 函数控制曲面和补片图形对象的颜色着色。

`shading flat `每个网格线段和面具有恒定颜色，该颜色由该线段的端点或该面的角边处具有最小索引的颜色值确定。

`shading faceted `具有叠加的黑色网格线的单一着色。这是默认的着色模式。

`shading interp `通过在每个线条或面中对颜色图索引或真彩色值进行插值来改变该线条或面中的颜色。

`shading(axes_handle,...) `将着色类型应用于 `axes_handle` 指定的坐标区而非当前坐标区中的对象。使用函数形式时，可以使用单引号。例如：

```
shading(gca,'interp')
```

##### mesh函数

用法

```matlab
mesh(x,y,z)
```

示例1

```matlab
x = -3:0.2:3;
y = 1:0.2:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,1)
mesh(X,Y,Z)
subplot(2,2,2)
mesh(X,Y,Z)
shading flat
 
x = -3:0.1:3;
y = 1:0.1:5;
[X,Y]=meshgrid(x,y);
Z = (X+Y).^2;
subplot(2,2,3)
mesh(X,Y,Z)
subplot(2,2,4)
mesh(X,Y,Z)
shading flat
```

![img](https://pic1.zhimg.com/80/v2-faad96f2a281bbb92364b55d33862d04_720w.jpg)

示例2

```matlab
m = 30;
z = 1.2*(0:m)/m;
r = ones(size(z));
theta = (0:m)/m*2*pi;
x1 = r'*cos(theta);y1=r'*sin(theta);
z1 = z'*ones(1,m+1);
x = (-m:2:m)/m;
x2 = x'*ones(1,m+1);y2=r'*cos(theta);
z2 = r'*sin(theta);
surf(x1,y1,z1);          
axis equal ,axis off
hold on
surf(x2,y2,z2);          
axis equal ,axis off
title ('Á½¸öµÈÖ±¾¶Ô²¹ÜµÄ½»Ïß');
hold off
```

![img](https://pic2.zhimg.com/80/v2-11a1f7997fffd7b34b16c3bfcf89c0f1_720w.jpg)

还有两个和mesh函数相似的函数，即带等高线的三维网格曲面函数meshc和带底座的三维网格曲面函数meshz，其用法和mesh类似。不同的是，meshc还在xy平面上绘制曲面在z轴方向的等高线，meshz还在xy平面上绘制曲面的底座。

surf函数也有两个类似的函数，即具有等高线的曲面函数surfc和具有光照效果的曲面函数surfl。

示例

```matlab
[x,y]=meshgrid(-8:0.5:8);
z=sin(sqrt(x.^2+y.^2))./sqrt(x.^2+y.^2+eps);
subplot(2,2,1);
meshc(x,y,z);
subplot(2,2,2);
meshz(x,y,z);
subplot(2,2,3);
surfc(x,y,z);
subplot(2,2,4);
surfl(x,y,z);
```

![img](https://pic2.zhimg.com/80/v2-33082b1950383caa24f4a63c46a2ed35_720w.jpg)

##### meshc函数

在网格图下显示等高线图 

创建三个相同大小的矩阵。然后将它们绘制为一个网格图，其下方有一个等高线图。网格图使用 `Z` 确定高度和颜色。

```
[X,Y] = meshgrid(-3:.125:3);
Z = peaks(X,Y);
meshc(X,Y,Z)
```

![Figure contains an axes. The axes contains 2 objects of type surface, contour.](https://ww2.mathworks.cn/help/matlab/ref/displaymeshandcontourplotofsurfaceexample_01_zh_CN.png)

#### 标准三维曲面

Matlab提供了一些函数用于绘制标准三维曲面，这些函数可以产生相应的绘图数据，常用于三维图形的演示。如，sphere函数和cylinder函数分别用于绘制三维球面和柱面。

用法

```matlab
[x,y,z]=sphere(n); %n越大，越平滑
[x,y,z]=cylinder(R,n)
```

示例

```matlab
[x,y,z]=sphere(100);
subplot(2,2,1)
mesh(x,y,z)
grid on
axis equal
subplot(2,2,2)
mesh(3*x,y,3*z)
axis equal
grid on
subplot(2,2,3)
mesh(5*x,y,z)
axis equal
grid on
subplot(2,2,4)
mesh(x,6*y,z)
axis equal
grid on
```

![img](https://pic1.zhimg.com/80/v2-2146bdc5f4d0038f96943855080356fc_720w.jpg)

示例2

```matlab
[x,y,z]=cylinder(5,200);
subplot(2,2,1)
mesh(x,y,z)
axis equal
grid on
subplot(2,2,2)
mesh(z,y,x)
axis equal
grid on
subplot(2,2,3)
mesh(x,5*y,5*z)
axis equal
grid on
t = 0:0.1:pi;
[x,y,z]=cylinder(sin(t),200);
subplot(2,2,4)
mesh(x,y,z)
axis equal
grid on
```

![img](https://pic1.zhimg.com/80/v2-179238e31de263407a06d40479fabdfc_720w.jpg)

#### 三维图形的精细处理：颜色、裁剪

Matlab定义的NaN常数可以用于表示那些不可使用的数据，利用这些特性，可以将图形中需要裁剪部分对应的函数值设置成NaN，这样在绘制图形时，函数值为NaN的部分将不显示出来，从而达到对图形进行裁剪的目的。例如，要削掉正弦波顶部或底部大于0.5的部分。

示例

```matlab
x=0:pi/50:4*pi;
y=sin(x);
i=find(abs(y)>0.5);
x(i)=NaN;
figure(1)
plot(x,y,'bo');
x1 = 0:pi/50:4*pi;
y1 = sin(x1);
hold on
plot(x1,y1,'r-')
```

![img](https://pic4.zhimg.com/80/v2-1d759645e086a0e80562189d66714417_720w.jpg)

示例

```matlab
[x,y,z]=sphere(25); %生成大球
z1=z;
z1(:,1:4)=NaN;%裁剪
c1=ones(size(z1)); %生成小球
surf(3*x,3*y,3*z1,c1);      
hold on
z2=z;
c2=2*ones(size(z2));
c2(:,1:4)=3*ones(size(c2(:,1:4)));
surf(1.5*x,1.5*y,1.5*z2,c2);
colormap([0 1 0;0.5 0 0;1 0 0]);
grid on
hold off
```

![img](https://pic3.zhimg.com/80/v2-bbe29b3297e4489520f319c0d002ac9e_720w.jpg)

#### 三维图形的精细处理:方位角

在日常生活中，从不同的角度观察物体，所看到的物体形状是不一样的。同样，从不同视点绘制的三维图形的形状也是不一样的。视点位置可由方位角和仰角表示。

Matlab提供了设置视点的函数view，其调用格式为：view(az,el) 其中az为方位角，el为仰角，它们均以度为单位。系统默认的视点定义为方位角为-37.5度，仰角30度。

示例

```matlab
subplot(2,2,1);mesh(peaks);
view(-37.5,30);
title('1');
subplot(2,2,2);mesh(peaks);
view(0,90);
title('2');
subplot(2,2,3);mesh(peaks);
view(90,0);
title('3');
subplot(2,2,4);mesh(peaks);
view(-7,-10);
title('4');
```

![img](https://pic4.zhimg.com/80/v2-592a839b71382cb37c47ba9f756fd9d3_720w.jpg)

