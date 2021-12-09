.. _gisprocess:


******************************
GIS处理
******************************

近邻匹配
================

| 下面的案例展示如何用TransBigData包进行点与点、点与线的近邻匹配。该方法使用的是KDTree算法，可查看wiki：https://en.wikipedia.org/wiki/K-d_tree，算法复杂度为o(log(n))


点与点匹配（DataFrame与DataFrame）
----------------------------------

| 导入TransBigData包

::

    import transbigdata as tbd

生成两个GeoDataFrame表，但它们只有经纬度列

::

    import pandas as pd
    import geopandas as gpd
    from shapely.geometry import LineString
    dfA = gpd.GeoDataFrame([[1,2],[2,4],[2,6],
                            [2,10],[24,6],[21,6],
                            [22,6]],columns = ['lon1','lat1'])
    dfB = gpd.GeoDataFrame([[1,3],[2,5],[2,2]],columns = ['lon','lat'])

使用tbd.ckdnearest进行点与点匹配，如果是DataFrame与DataFrame匹配（不含有地理信息），则需要指定前后两个表的经纬度列


.. function:: transbigdata.ckdnearest(dfA_origin,dfB_origin,Aname = ['lon','lat'],Bname = ['lon','lat'])

输入两个DataFrame，分别指定经纬度列名，为表A匹配表B中最近点，并计算距离

**输入**

dfA_origin : DataFrame
    表A
dfB_origin : DataFrame
    表B
Aname : List
    表A中经纬度列字段
Bname : List
    表B中经纬度列字段

**输出**

gdf : DataFrame
    为A匹配到B上最近点的表

::

    tbd.ckdnearest(dfA,dfB,Aname=['lon1','lat1'],Bname=['lon','lat'])
    # The calculated distance at this time is the actual distance 
    # converted from latitude and longitude




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>lon1</th>
          <th>lat1</th>
          <th>index</th>
          <th>lon</th>
          <th>lat</th>
          <th>dist</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>1</td>
          <td>2</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>1.111949e+05</td>
        </tr>
        <tr>
          <th>1</th>
          <td>2</td>
          <td>4</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>1.111949e+05</td>
        </tr>
        <tr>
          <th>2</th>
          <td>2</td>
          <td>6</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>1.111949e+05</td>
        </tr>
        <tr>
          <th>3</th>
          <td>2</td>
          <td>10</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>5.559746e+05</td>
        </tr>
        <tr>
          <th>4</th>
          <td>24</td>
          <td>6</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>2.437393e+06</td>
        </tr>
        <tr>
          <th>5</th>
          <td>21</td>
          <td>6</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>2.105798e+06</td>
        </tr>
        <tr>
          <th>6</th>
          <td>22</td>
          <td>6</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>2.216318e+06</td>
        </tr>
      </tbody>
    </table>
    </div>



点与点匹配（GeoDataFrame与GeoDataFrame）
----------------------------------------

将A表B表变为含有点信息的GeoDataFrame

::

    dfA['geometry'] = gpd.points_from_xy(dfA['lon1'], dfA['lat1'])
    dfB['geometry'] = gpd.points_from_xy(dfB['lon'], dfB['lat'])

使用tbd.ckdnearest_point进行点与点匹配

.. function:: transbigdata.ckdnearest_point(gdA, gdB)

输入两个GeoDataFrame，gdfA、gdfB均为点，该方法会为gdfA表连接上gdfB中最近的点，并添加距离字段dsit

**输入**

gdA : GeoDataFrame
    表A，点要素
gdB : GeoDataFrame
    表B，点要素

**输出**

gdf : GeoDataFrame
    为A匹配到B上最近点的表


::

    tbd.ckdnearest_point(dfA,dfB)
    # The calculated distance is the latitude-andlongitude-based distance




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>lon1</th>
          <th>lat1</th>
          <th>geometry_x</th>
          <th>dist</th>
          <th>index</th>
          <th>lon</th>
          <th>lat</th>
          <th>geometry_y</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>1</td>
          <td>2</td>
          <td>POINT (1.00000 2.00000)</td>
          <td>1.000000</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>POINT (1.00000 3.00000)</td>
        </tr>
        <tr>
          <th>1</th>
          <td>2</td>
          <td>4</td>
          <td>POINT (2.00000 4.00000)</td>
          <td>1.000000</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
        <tr>
          <th>2</th>
          <td>2</td>
          <td>6</td>
          <td>POINT (2.00000 6.00000)</td>
          <td>1.000000</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
        <tr>
          <th>3</th>
          <td>2</td>
          <td>10</td>
          <td>POINT (2.00000 10.00000)</td>
          <td>5.000000</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
        <tr>
          <th>4</th>
          <td>24</td>
          <td>6</td>
          <td>POINT (24.00000 6.00000)</td>
          <td>22.022716</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
        <tr>
          <th>5</th>
          <td>21</td>
          <td>6</td>
          <td>POINT (21.00000 6.00000)</td>
          <td>19.026298</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
        <tr>
          <th>6</th>
          <td>22</td>
          <td>6</td>
          <td>POINT (22.00000 6.00000)</td>
          <td>20.024984</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>POINT (2.00000 5.00000)</td>
        </tr>
      </tbody>
    </table>
    </div>



点与线匹配（GeoDataFrame与GeoDataFrame）
----------------------------------------

将A表变为地理点，B表为线

::

    dfA['geometry'] = gpd.points_from_xy(dfA['lon1'],dfA['lat1'])
    dfB['geometry'] = [LineString([[1,1],[1.5,2.5],[3.2,4]]),
                      LineString([[1,0],[1.5,0],[4,0]]),
                       LineString([[1,-1],[1.5,-2],[4,-4]])]
    dfB.plot()


.. image:: _static/output_15_1.png



.. function:: transbigdata.ckdnearest_line(gdfA, gdfB)

输入两个GeoDataFrame，其中gdfA为点，gdfB为线，该方法会为gdfA表连接上gdfB中最近的线，并添加距离字段dsit

**输入**

gdA : GeoDataFrame
    表A，点要素
gdB : GeoDataFrame
    表B，线要素

**输出**

gdf : GeoDataFrame
    为A匹配到B中最近的线

用tbd.ckdnearest_line可以实现点匹配线，其原理是将线中的折点提取，然后使用点匹配点。

::

    tbd.ckdnearest_line(dfA,dfB)
    # The calculated distance is the latitude-andlongitude-based distance




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>lon1</th>
          <th>lat1</th>
          <th>geometry_x</th>
          <th>dist</th>
          <th>index</th>
          <th>lon</th>
          <th>lat</th>
          <th>geometry_y</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>1</td>
          <td>2</td>
          <td>POINT (1.00000 2.00000)</td>
          <td>0.707107</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>1</th>
          <td>2</td>
          <td>4</td>
          <td>POINT (2.00000 4.00000)</td>
          <td>1.200000</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>2</th>
          <td>2</td>
          <td>6</td>
          <td>POINT (2.00000 6.00000)</td>
          <td>2.332381</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>3</th>
          <td>2</td>
          <td>10</td>
          <td>POINT (2.00000 10.00000)</td>
          <td>6.118823</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>4</th>
          <td>21</td>
          <td>6</td>
          <td>POINT (21.00000 6.00000)</td>
          <td>17.912007</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>5</th>
          <td>22</td>
          <td>6</td>
          <td>POINT (22.00000 6.00000)</td>
          <td>18.906084</td>
          <td>0</td>
          <td>1</td>
          <td>3</td>
          <td>LINESTRING (1.00000 1.00000, 1.50000 2.50000, ...</td>
        </tr>
        <tr>
          <th>6</th>
          <td>24</td>
          <td>6</td>
          <td>POINT (24.00000 6.00000)</td>
          <td>20.880613</td>
          <td>1</td>
          <td>2</td>
          <td>5</td>
          <td>LINESTRING (1.00000 0.00000, 1.50000 0.00000, ...</td>
        </tr>
      </tbody>
    </table>
    </div>






打断线
===============

在实际应用中，我们可能会需要把很长的线打断为很多子线段，每一条线段不要超过一定的最大长度，此时则可以使用TransBigData包中的splitline_with_length方法。


.. function:: transbigdata.splitline_with_length(Centerline,maxlength = 100)

输入线GeoDataFrame要素，打断为最大长度maxlength的小线段

**输入**

Centerline : GeoDataFrame
    线要素
maxlength : number
    打断的线段最大长度

**输出**

splitedline : GeoDataFrame
    打断后的线

下面演示如何将线打断为100米一段的线段

::

    # Read the line GeoDataFrame
    import geopandas as gpd
    Centerline = gpd.read_file(r'test_lines.json')
    Centerline.plot()





.. image:: splitline/output_2_1.png


::

    # Convert line to projected coordinate system
    Centerline.crs = {'init':'epsg:4326'}
    Centerline = Centerline.to_crs(epsg = '4517')
    
    # calculate the line's length
    Centerline['length'] = Centerline.length
    Centerline




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Id</th>
          <th>geometry</th>
          <th>length</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>0</td>
          <td>LINESTRING (29554925.232 4882800.694, 29554987...</td>
          <td>285.503444</td>
        </tr>
        <tr>
          <th>1</th>
          <td>0</td>
          <td>LINESTRING (29554682.635 4882450.554, 29554773...</td>
          <td>185.482276</td>
        </tr>
        <tr>
          <th>2</th>
          <td>0</td>
          <td>LINESTRING (29554987.079 4882521.969, 29555040...</td>
          <td>291.399180</td>
        </tr>
        <tr>
          <th>3</th>
          <td>0</td>
          <td>LINESTRING (29554987.079 4882521.969, 29555073...</td>
          <td>248.881529</td>
        </tr>
        <tr>
          <th>4</th>
          <td>0</td>
          <td>LINESTRING (29554987.079 4882521.969, 29554969...</td>
          <td>207.571197</td>
        </tr>
        <tr>
          <th>5</th>
          <td>0</td>
          <td>LINESTRING (29554773.177 4882288.671, 29554828...</td>
          <td>406.251357</td>
        </tr>
        <tr>
          <th>6</th>
          <td>0</td>
          <td>LINESTRING (29554773.177 4882288.671, 29554926...</td>
          <td>158.114403</td>
        </tr>
        <tr>
          <th>7</th>
          <td>0</td>
          <td>LINESTRING (29555060.286 4882205.456, 29555082...</td>
          <td>107.426629</td>
        </tr>
        <tr>
          <th>8</th>
          <td>0</td>
          <td>LINESTRING (29555040.278 4882235.468, 29555060...</td>
          <td>36.069941</td>
        </tr>
        <tr>
          <th>9</th>
          <td>0</td>
          <td>LINESTRING (29555060.286 4882205.456, 29555095...</td>
          <td>176.695446</td>
        </tr>
      </tbody>
    </table>
    </div>



::

    # split the line to maximum 100m
    import transbigdata as tbd
    splitedline = tbd.splitline_with_length(Centerline,maxlength = 100)

::

    # the splited line remain unchanged
    splitedline.plot()








.. image:: splitline/output_5_1.png


::

    # it does been splited
    splitedline




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>geometry</th>
          <th>id</th>
          <th>length</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554925.232 4882800.694, 29554927...</td>
          <td>0</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29554946.894 4882703.068, 29554949...</td>
          <td>0</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>2</th>
          <td>LINESTRING (29554968.557 4882605.443, 29554970...</td>
          <td>0</td>
          <td>85.503444</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554682.635 4882450.554, 29554688...</td>
          <td>1</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29554731.449 4882363.277, 29554736...</td>
          <td>1</td>
          <td>85.482276</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554987.079 4882521.969, 29554989...</td>
          <td>2</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29555005.335 4882423.650, 29555007...</td>
          <td>2</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>2</th>
          <td>LINESTRING (29555023.592 4882325.331, 29555025...</td>
          <td>2</td>
          <td>91.399180</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554987.079 4882521.969, 29554993...</td>
          <td>3</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29555042.051 4882438.435, 29555048...</td>
          <td>3</td>
          <td>99.855617</td>
        </tr>
        <tr>
          <th>2</th>
          <td>LINESTRING (29555111.265 4882370.450, 29555116...</td>
          <td>3</td>
          <td>48.881529</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554987.079 4882521.969, 29554985...</td>
          <td>4</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29554973.413 4882422.908, 29554971...</td>
          <td>4</td>
          <td>99.756943</td>
        </tr>
        <tr>
          <th>2</th>
          <td>LINESTRING (29554930.341 4882335.023, 29554929...</td>
          <td>4</td>
          <td>7.571197</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554773.177 4882288.671, 29554777...</td>
          <td>5</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29554816.361 4882198.476, 29554821...</td>
          <td>5</td>
          <td>99.782969</td>
        </tr>
        <tr>
          <th>2</th>
          <td>LINESTRING (29554882.199 4882125.314, 29554891...</td>
          <td>5</td>
          <td>99.745378</td>
        </tr>
        <tr>
          <th>3</th>
          <td>LINESTRING (29554976.612 4882096.588, 29554987...</td>
          <td>5</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>4</th>
          <td>LINESTRING (29555076.548 4882100.189, 29555077...</td>
          <td>5</td>
          <td>6.251357</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29554773.177 4882288.671, 29554783...</td>
          <td>6</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29554869.914 4882314.006, 29554876...</td>
          <td>6</td>
          <td>58.114403</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29555060.286 4882205.456, 29555062...</td>
          <td>7</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29555081.239 4882107.675, 29555081...</td>
          <td>7</td>
          <td>7.426629</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29555040.278 4882235.468, 29555042...</td>
          <td>8</td>
          <td>36.069941</td>
        </tr>
        <tr>
          <th>0</th>
          <td>LINESTRING (29555060.286 4882205.456, 29555064...</td>
          <td>9</td>
          <td>100.000000</td>
        </tr>
        <tr>
          <th>1</th>
          <td>LINESTRING (29555094.981 4882299.244, 29555100...</td>
          <td>9</td>
          <td>76.419694</td>
        </tr>
      </tbody>
    </table>
    </div>

面合并
========================

.. function:: transbigdata.merge_polygon(data,col)

输入多边形GeoDataFrame数据，以及分组列名col，对不同组别进行分组的多边形进行合并

**输入**

data : GeoDataFrame
    多边形数据
col : str
    分组列名

**输出**

data1 : GeoDataFrame
    合并后的面


对面取外边界构成新多边形
================================================


.. function:: transbigdata.polyon_exterior(data,minarea = 0)

输入多边形GeoDataFrame数据，对多边形取外边界构成新多边形

**输入**

data : GeoDataFrame
    多边形数据
minarea : number
    最小面积，小于这个面积的面全部剔除
    
**输出**

data1 : GeoDataFrame
    处理后的面
