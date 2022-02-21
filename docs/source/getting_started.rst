.. _getting_started:


******************************
安装、依赖与更新日志
******************************

安装
=============================

It is recommended to use `Python 3.7, 3.8, 3.9`

TransBigData依赖geopandas，在安装TransBigData前需要根据 `这个链接 <https://geopandas.org/en/stable/getting_started.html#installation>`_ 中的方法安装geopandas。
如果你已经安装了geopandas，则直接在命令提示符中运行下面代码即可安装::

  pip install -U transbigdata


You can also install `TransBigData` by conda-forge, this will automaticaly solve the dependency, it can be installed with::

  conda install -c conda-forge transbigdata

在Python中运行下面代码::

  import transbigdata as tbd

依赖包
=============================
TransBigData依赖如下包

* `pandas`
* `shapely`
* `rtree`
* `geopandas`
* `scipy`
* `matplotlib`
* `networkx` (optional)
* `igraph` (optional)
* `osmnx` (optional)
* `seaborn` (optional)
* `keplergl` (optional)
