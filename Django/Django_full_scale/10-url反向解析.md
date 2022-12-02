### url反向解析

[2.06-url反向解析_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vK4y1o7jH?p=12&vd_source=2cbd78fa4c29853f276b2711bac1e01c)

url反向解析是指在视图或模板中，用path定义的名称来动态查找或计算出相应的路由

path函数用法：

- path(route, views, name="别名")
- path('page', views.page_view, name="page_url")

根据path中的'name='关键字传参给 url 确定了一个唯一确定的名字，在模板或视图中，可以通过这个名字反向推断出此url