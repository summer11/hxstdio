title: 诗一样的javascript语句
date: 2014-12-03 14:30:56
tags:
- javascript
categories: 前端学习笔记
---
以下搬运自前淘宝前端 @流火 大神的 [笨笨剥壳](http://www.benben.cc/blog/?p=391)

#### 1.将一个数组插入另一个数组的指定位置
```{javascript}
var a = [1,2,3,7,8,9];
var b = [4,5,6];
var insertIndex = 3;
a.splice.apply(a, Array.concat(insertIndex, 0, b)); // a: 1,2,3,4,5,6,7,8,9
```

#### 2.删除数组元素
```{javascript}
var a = [1,2,3,4,5];
a.splice(3,1); //从index为3的元素开始，删除1个元素
```

#### 3.快速获取最大值、最小值
```{javascript}
Math.max.apply(Math, [1,2,3]) //3
Math.min.apply(Math, [1,2,3]) //1
```

#### 4.条件判断
```{javascript}
var a = b && 1;
//相当于
if (b) {
    a = 1;
} else {
    a = b;
}
 
var a = b || 1; 
//相当于
if (b) {
    a = b;
} else {
    a = 1;
}
```

#### 5.取整同时转成数值型
```{javascript}
'10.567890'|0 //结果: 10
'10.567890'^0 //结果: 10
-2.23456789|0 //结果: -2
~~-2.23456789 //结果: -2
```

#### 6.日期转成数值
```{javascript}
var d = +new Date(); //1295698416792
```

#### 7.类数组对象转数组
```{javascript}
var arr = [].slice.call(arguments)
```

#### 8.漂亮的随机码
```{javascript}
Math.random().toString(16).substring(2); //14位
Math.random().toString(36).substring(2); //11位
```

#### 9.合并数组
```{javascript}
var a = [1,2,3];
var b = [4,5,6];
Array.prototype.push.apply(a, b);
uneval(a); //[1,2,3,4,5,6]
```

#### 10.用0补全位数
```{javascript}
function prefixInteger(num, length) {
    return (num / Math.pow(10, length)).toFixed(length).substr(2);
}
prefixInteger(125,6); //000125
```

#### 11.交换值
```{javascript}
a= [b, b=a][0];
```