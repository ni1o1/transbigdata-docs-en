# TransBigData 针对交通时空大数据处理的Python包

<img src="https://github.com/ni1o1/transbigdata/raw/main/docs/source/_static/logo-wordmark-dark.png" style="width:550px">

[![Documentation Status](https://readthedocs.org/projects/transbigdata/badge/?version=latest)](https://transbigdata.readthedocs.io/en/latest/?badge=latest) ![PyPI](https://img.shields.io/pypi/v/transbigdata) [![bilibili](https://img.shields.io/badge/bilibili-%E5%90%8C%E6%B5%8E%E5%B0%8F%E6%97%AD%E5%AD%A6%E9%95%BF-green.svg)](https://space.bilibili.com/3051484)  


**主要功能**

TransBigData工具针对时空大数据处理而开发，依托于GeoPandas。TransBigData集成了交通时空大数据处理过程中常用的方法。包括栅格化、数据质量分析、数据预处理、数据集计、轨迹分析、GIS处理、地图底图加载、坐标与距离计算、数据可视化等通用方法。TransBigData也针对出租车GPS数据、共享单车数据、公交GPS数据等多种常见交通时空大数据提供了快速简洁的处理方法。

**技术特点**

* 面向交通时空大数据分析不同阶段的处理需求提供不同处理功能。
* 代码简洁、高效、灵活、易用，通过简短的代码即可实现复杂的数据任务。


更多细节请查看：[TransBigData的说明文档](https://transbigdata.readthedocs.io/)


## 安装

在安装TransBigData之前，请确保已经安装了可用的geopandas包：https://geopandas.org/index.html  
如果你已经安装了geopandas，则直接在命令提示符中运行下面代码即可安装

    pip install -U transbigdata


## 使用

下面例子展示如何使用TransBigData工具快速地从出租车GPS数据中提取出行OD:

    #导入TransBigData包
    import transbigdata as tbd
    #读取数据    
    import pandas as pd
    data = pd.read_csv('TaxiData-Sample.csv',header = None) 
    data.columns = ['VehicleNum','time','slon','slat','OpenStatus','Speed'] 
    data

<img src="https://github.com/ni1o1/transbigdata/raw/main/docs/source/_static/WX20211021-192131@2x.png" style="height:300px">

使用transbigdata.taxigps_to_od方法，传入对应的列名，即可提取出行OD:

    #从GPS数据提取OD
    oddata = tbd.taxigps_to_od(data,col = ['VehicleNum','time','slon','slat','OpenStatus'])
    oddata

<img src="https://github.com/ni1o1/transbigdata/raw/main/docs/source/_static/WX20211021-190104@2x.png" style="height:300px">

对提取出的OD进行OD的栅格集计:

    #定义研究范围
    bounds = [113.6,22.4,114.8,22.9]
    #输入研究范围边界bounds与栅格宽度accuracy，获取栅格化参数
    params = tbd.grid_params(bounds = bounds,accuracy = 1500)
    #栅格化OD并集计
    od_gdf = tbd.odagg_grid(oddata,params)
    od_gdf.plot(column = 'count')

<img src="https://github.com/ni1o1/transbigdata/raw/main/docs/source/_static/WX20211021-190524@2x.png" style="height:300px">

## 相关链接

* 小旭学长的b站： https://space.bilibili.com/3051484
* 小旭学长的七天入门交通时空大数据分析课程（零基础免费课）： https://www.lifangshuju.com/#/introduce/166  
* 小旭学长的交通时空大数据分析课程： https://www.lifangshuju.com/#/introduce/154  
* 小旭学长的数据可视化课程： https://www.lifangshuju.com/#/introduce/165  
* 本项目的github页面： https://github.com/ni1o1/transbigdata/  
* 有bug请在这个页面提交： https://github.com/ni1o1/transbigdata/issues  

## 引用

而如果你想要引用这个 GitHub 仓库，可以使用如下的 bibtex：

```
@misc{transbigdata,
  author = {Qing Yu},
  title = {TransBigData},
  year = {2021},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/ni1o1/transbigdata}},
}
```