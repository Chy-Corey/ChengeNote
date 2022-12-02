# ECharts

那么什么是echarts呢？ECharts是一款开源、功能强大的数据可视化产品，它是由百度团开发创建的前端可视化工具。具有很强大的功能。入门也及其的简单。ECharts 是一个使用 JavaScript 实现的开源可视化库，涵盖各行业图表，满足各种需求。

### 1. Get Started with ECharts

#### (1) Install & Include ECharts

Using npm: `npm install echarts --save`. 

Load `echarts.min.js` with a script tag.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- including ECharts file -->
    <script src="echarts.min.js"></script>
</head>
</html>
```

#### (2) Draw a simple chart

The chart we wanna draw need to be placed in a box(Dom container) with height and width.

```html
<body>
    <!-- preparing a DOM with width and height for ECharts -->
    <div id="main" style="width:600px; height:400px;"></div>
</body>
```

Then we can initialize an ECharts instance using `echarts init`, and create a simple bar chart with `setOption`. Of course, we need to prepare the option.Below is the complete code.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ECharts</title>
    <!-- including ECharts file -->
    <script src="echarts.js"></script>
</head>
<body>
    <!-- prepare a DOM container with width and height -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
        // based on prepared DOM, initialize echarts instance
        var myChart = echarts.init(document.getElementById('main'));

        // specify chart configuration item and data
        var option = {
            title: {
                text: 'ECharts entry example'
            },
            tooltip: {},
            legend: {
                data:['Sales']
            },
            xAxis: {
                data: ["shirt","cardign","chiffon shirt","pants","heels","socks"]
            },
            yAxis: {},
            series: [{
                name: 'Sales',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        };

        // use configuration item and data specified to show chart
        myChart.setOption(option);
    </script>
</body>
</html>
```

Now we have made our first EChart!

### 2. Different types of charts

On the official website, we can find different examples. We just need to browse these examples and copy the code.

[Examples - Apache ECharts](https://echarts.apache.org/examples/en/index.html)

### 3. Related Configeration

ECharts has lots of configeration, what we can do is browsing the teaching docs. Below is the link.

[Documentation - Apache ECharts](https://echarts.apache.org/en/option.html#title)

Next we will choose another example (Stacked Line Chart) to change some configeration.

![image-20210811144727573](C:\Users\陈鸿煜\AppData\Roaming\Typora\typora-user-images\image-20210811144727573.png)

I will show some important attributes. To learn more detailed attributes, we can consult the official docs.

---

#### (1) Title

Title component, including main title and subtitle.

|   key   |       value       |                        function                        |
| :-----: | :---------------: | :----------------------------------------------------: |
|  text   |      String       | The main title text, supporting for `\n` for newlines. |
|  link   |      String       |           The hyper link of main title text.           |
| target  | 'self' or 'blank' |  Open the hyper link of main title in specified tab.   |
| subtext |      String       |    Subtitle text, supporting for `\n` for newlines.    |

#### (2) Legend

Legend component.

Legend component shows symbol, color and name of different series. You can click legends to toggle displaying series in the chart. In ECharts 3, a single echarts instance may contain multiple legend components, which makes it easier for the layout of multiple legend components. If there have to be too many legend items, [vertically scrollable legend](https://echarts.apache.org/examples/en/editor.html?c=pie-legend&edit=1&reset=1) or [horizontally scrollable legend](https://echarts.apache.org/examples/en/editor.html?c=radar2&edit=1&reset=1) are options to paginate them. Check [legend.type](https://echarts.apache.org/en/option.html#legend.type) please.

|     key      |           value            |                           function                           |
| :----------: | :------------------------: | :----------------------------------------------------------: |
|     type     |    'scroll' or 'plain'     |                       Type of legend.                        |
|     left     |           String           | Distance between legend component and the left side of the container. |
|    right     |           String           |                                                              |
|     top      |           String           |                                                              |
|    bottom    |           String           |                                                              |
|    orient    | 'horizontal' or 'vertical' |              The layout orientation of legend.               |
|   itemGap    |           number           |              The distance between each legend.               |
| selectedMode |          Boolean           | Selected mode of legend, which controls whether series can be toggled displaying by clicking legends. |
|     data     |                            |                                                              |

data attribute is very complex, we can only consult the official docs.

#### (3) Grid

Drawing grid in rectangular coordinate.
