---
title: Highcharts使用基础
categories:
  - 前端
tags:
  - HighCharts
author: geepair
date: 2020-03-21 21:08:00
---

前端开发一定少不了应用外部的依赖文件，刚开始学习 `highcharts` ，使用 `Postman` 来测试一些cdn的get请求方式的响应耗时，故使用 `highcharts` 来直观的显示统计的数据。

<!-- more -->

# HighChart的基本使用

- 使用 `highcharts` 必须引用两个额外的文件： jquery和highcharts的js文件。

```javascript
<script src="https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdn.staticfile.org/highcharts/8.0.4/highcharts.min.js"></script>
```

当然，也可以把文件下载到本地，本地引用就行了。

- 使用 `highcharts` 非常简单

```html
<div id="contianer" style="width: 550px; height: 400px; "></div>
<script>
$().ready(function(){
    $('#contianer').highcharts(json);
});
</script>
```

其中 `container`选择一个放置图标的容器, `json` 是 图标的配置文件和数据对象。

- 使用 `Postman` 进行 `get` 请求 ，控制同一个文件的版本相同，对几个 CDN 进行测试。各个cdn分别迭代10次，延迟1s，期间对出入较大的进行舍弃，找出一组波动不太大的数据。

- 把统计数据记录好，填充至图表中。

highcharts `json` 是一个对象，通过对不同的属性进行配置来控制图标的生成，以下是一些基本属性

* title: 图表的标题
* subtitle: 图标的子标题
* xAxis: x轴
* yAxis: y轴
* tooltip: 提示工具
* legend: 图标类型
* cerdits：这个可以把highcharts的水印去除
* series: 用来填充数据
* ......

#### 统计

- 以下是把数据填充进图标后形成的折线图。

<div id="jquery" style="min-width: 300px; height: 300px;"></div>
<div id="bootstrap" style="min-width: 300px; height: 300px;"></div>
<div id="angular" style="min-width: 300px; height: 300px;"></div>
<div id="highcharts" style="min-width: 300px; height: 300px;"></div>
<script src="https://cdn.staticfile.org/highcharts/8.0.4/highcharts.min.js"></script>
<script>
        // $(function () {
        let jqueryJson = {
            chart: {
                renderTo: 'jquery'
            },
            title: {
                text: 'jQuery v1.3.1'
            },
            subtitle: {
                text: 'Source: ijava.icu',
            },
            xAxis: {
                categories: ['第1次', '第2次', '第3次', '第4次', '第5次', '第6次', '第7次', '第8次', '第9次', '第10次',
                    '平均耗时'
                ]
            },
            yAxis: {
                title: {
                    text: 'Speed (ms)',
                },
            },
            tooltip: {
                valueSuffix: 'ms'
            },
            legend: {
                layout: 'vertical',
                align: 'right',
                verticalAlign: 'middle',
                borderWidth: 0
            },
            credits: {
                //去除logo
                enabled: false
            },
            series: [{
                    name: '新浪CDN',
                    data: [101, 103, 100, 90, 99, 101, 102, 100, 100, 99, 99.5]
                },
                {
                    name: 'BootCDN',
                    data: [61, 64, 57, 61, 58, 64, 60, 58, 60, 62, 60.5]
                },
                {
                    name: '七牛云',
                    data: [31, 28, 30, 28, 30, 30, 27, 26, 28, 25, 28.3]
                },
                {
                    name: '又拍云',
                    data: [48, 37, 41, 168, 33, 42, 37, 32, 120, 35, 59.3]
                }
            ],
  responsive: {
                rules: [{
                    condition: {
                        maxWidth: 500
                    },
                    chartOptions: {
                        legend: {
                            enabled: false
                        }
                    }
                }]
            }
        };
        new Highcharts.Chart(jqueryJson);
        let bootstrapJson = {
            chart: {
                renderTo: 'bootstrap'
            },
            title: {
                text: 'BootStrap v3.1.0'
            },
            subtitle: {
                text: 'Source: ijava.icu',
            },
            xAxis: {
                categories: ['第1次', '第2次', '第3次', '第4次', '第5次', '第6次', '第7次', '第8次', '第9次', '第10次',
                    '平均耗时'
                ]
            },
            yAxis: {
                title: {
                    text: 'Speed (ms)',
                },
            },
            tooltip: {
                valueSuffix: 'ms'
            },
            legend: {
                layout: 'vertical',
                align: 'right',
                verticalAlign: 'middle',
                borderWidth: 0
            },
            credits: {
                //去除logo
                enabled: false
            },
            series: [{
                    name: '新浪CDN',
                    data: [102, 105, 104, 104, 105, 109, 104, 104, 106, 105, 104.8]
                },
                {
                    name: 'BootCDN',
                    data: [133, 137, 127, 131, 132, 128, 141, 135, 133, 125, 132.2]
                },
                {
                    name: '七牛云',
                    data: [41, 29, 29, 42, 42, 51, 39, 42, 28, 30, 37.3]
                }
            ],
  responsive: {
                rules: [{
                    condition: {
                        maxWidth: 500
                    },
                    chartOptions: {
                        legend: {
                            enabled: false
                        }
                    }
                }]
            }
        };
        new Highcharts.Chart(bootstrapJson);
        let angularJson = {
            chart: {
                renderTo: 'angular'
            },
            title: {
                text: 'Angular v1.2.19'
            },
            subtitle: {
                text: 'Source: ijava.icu',
            },
            xAxis: {
                categories: ['第1次', '第2次', '第3次', '第4次', '第5次', '第6次', '第7次', '第8次', '第9次', '第10次',
                    '平均耗时'
                ]
            },
            yAxis: {
                title: {
                    text: 'Speed (ms)',
                },
            },
            tooltip: {
                valueSuffix: 'ms'
            },
            legend: {
                layout: 'vertical',
                align: 'right',
                verticalAlign: 'middle',
                borderWidth: 0
            },
            credits: {
                //去除logo
                enabled: false
            },
            series: [{
                    name: '新浪CDN',
                    data: [396, 394, 394, 394, 396, 399, 398, 399, 388, 393, 395.1]
                },
                {
                    name: 'BootCDN',
                    data: [67, 59, 61, 64, 73, 62, 65, 65, 64, 59, 63.9]
                },
                {
                    name: '七牛云',
                    data: [31, 28, 31, 32, 28, 29, 29, 29, 32, 29, 29.8]
                }
            ],
  responsive: {
                rules: [{
                    condition: {
                        maxWidth: 500
                    },
                    chartOptions: {
                        legend: {
                            enabled: false
                        }
                    }
                }]
            }
  };
        new Highcharts.Chart(angularJson);
        let highchartsJson = {
            chart: {
                renderTo: 'highcharts'
            },
            title: {
                text: 'HighCharts v4.0.1'
            },
            subtitle: {
                text: 'Source: ijava.icu',
            },
            xAxis: {
                categories: ['第1次', '第2次', '第3次', '第4次', '第5次', '第6次', '第7次', '第8次', '第9次', '第10次',
                    '平均耗时'
                ]
            },
            yAxis: {
                title: {
                    text: 'Speed (ms)',
                },
            },
            tooltip: {
                valueSuffix: 'ms'
            },
            legend: {
                layout: 'vertical',
                align: 'right',
                verticalAlign: 'middle',
                borderWidth: 0
            },
            credits: {
                //去除logo
                enabled: false
            },
            series: [{
                    name: '新浪CDN',
                    data: [165, 157, 162, 160, 158, 154, 159, 161, 157, 164, 159.7]
                },
                {
                    name: 'BootCDN',
                    data: [80, 76, 77, 80, 81, 80, 75, 81, 80, 79, 78.9]
                },
                {
                    name: '七牛云',
                    data: [55, 33, 34, 34, 39, 31, 33, 30, 51, 31, 37.1]
                }
            ],
  responsive: {
                rules: [{
                    condition: {
                        maxWidth: 500
                    },
                    chartOptions: {
                        legend: {
                            enabled: false
                        }
                    }
                }]
            }
        };
        new Highcharts.Chart(highchartsJson);
        // })
    </script>

#### 6. 结论

因为选择的cdn都是国内的免费cdn，数量比较少，得出的结果不太具有说服力，只能给出一个推荐：
[七牛云](http://www.staticfile.org/) 是我觉得比较好的一个，所有的速度都比较块，波动也不太明显。
[BootCDN](https://www.bootcdn.cn/) 速度也还行。
[又拍云JS库](http://jscdn.upai.com/) 只有少量的JavaScript文件，但是速度还行。
[新浪CDN](http://lib.sinaapp.com/) 速度就比较慢了，不推荐。