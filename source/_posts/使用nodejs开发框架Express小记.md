title: 使用nodejs的web开发框架Express小记
date: 2014-11-25 11:36:21
tags:
- nodejs
categories: 
---
好记性不如烂笔头~

前一阵子为组内搭建了一个线上接口的响应时间分析系统，用的是nodejs作为服务端、Express作为框架、数据库使用mysql、前端展示用了bootstrap、以及hightcharts用来生成各种漂亮的图表。期中踩了不少坑，时间久了怕忘记，记录下来方便以后查阅。

生成的图表大概长这样…敏感数据打码。

![图一](/images/goodsample.png)
![图二](/images/badsample.png)

- 横坐标：接口响应时间
- 纵坐标：响应时间区间内的请求数量

表现正常的接口，其接口时间应该是一个正态分布图，如图一。

而图二，明显的有两个波峰，说明接口在特定场景下，响应时间会变长。体现在业务上会发现某个菊花有时候会转好久，这就需要反馈给服务端同学去查原因了。

#### 一、环境搭建

主要是参考了这篇文章 : [Nodejs开发框架Express3.0开发手记–从零开始](http://blog.fens.me/nodejs-express3/)

#### 二、笔记

###### 1.Express生成工程的目录结构：
<img style="height:100px;" src="/images/express.png" />

- doc：无视掉，自己写的文档
- node_modules：无视掉，存放所有的项目依赖库
- public：静态文件
    + img：存放html页面中引用的图片，bootstrap中引用的图片直接扔这就行
    + javascript：引用的js库，比如jquery.js,bootstrap.js,hightcharts.js等，以及html页面中引用的index.js等
    + stylesheets：引用的css库，比如bootstrap.css等
- routes：路由文件(MVC中的C)
    + index.js：首页
    + ajaxRouter.js：处理所有ajax请求
    + utils：处理数据库请求、公共方法、存放静态变量等
- views：存放html页面,ejs模板
- app.js：程序启动文件，服务端相关配置

###### 2.代码片段

- 配置路由，指定处理ajax请求的具体方法：
```{javascript}
app.js

var ajaxRoutes = require('./routes/ajaxRoutes');
app.post('/queryApiName', ajaxRoutes.queryApiName); 
app.post('/queryApiCost', ajaxRoutes.queryApiCost);
app.post('/queryBussinessList', ajaxRoutes.queryBussinessList); 
app.post('/queryMtopList', ajaxRoutes.queryMtopList);
```

- 从前端页面发送ajax请求数据库：
```{javascript}
index.js

    $.ajax({
        type: 'POST',
        dataType: 'json',
        url: '/queryApiCost',
        data: {api : apiName}, //传参，在处理ajax请求的方法中，通过req.body.api获取
        success: function (data) {
            // 拿到数据后，更新前端页面
            $('#div_chart').show();
            _drawLineCharts(data, '#container_line_chart');
            _drawBarCharts(data, '#container_bar_chart');
        },error:function(XMLHttpRequest, textStatus, errorThrown){
            //TODO: 统一出错页面
        }
    });
```

- 处理ajax请求：
```{javascript}
ajaxRouter.js

// 处理AJAX请求，获取所有有数据的MTOP接口名称，并以JSON形式返回给前端 
exports.queryApiName = function(req, res){
    console.log(req.body.input); //ajax请求中的传递来的参数，data:{input:"xxxx"}

    dbHandler.query_all_apis_name(function(rows_api_names){
        var apiNameList = dataHandler.getApiNameList(rows_api_names);

        //转成json格式 
        res.send(JSON.stringify(apiNameList));
    });
};

```

