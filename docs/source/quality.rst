.. _quality:


******************************
数据质量分析
******************************



.. function:: transbigdata.data_summary(data,col = ['Vehicleid','Time'],show_sample_duration = False)

输入数据，打印数据概况

**输入**

data : DataFrame
    轨迹点数据
col : List
    列名，按[个体ID，时间]的顺序
show_sample_duration : bool
    是否输出个体采样间隔信息
roundnum : number
    小数点取位数
    
使用方法

::

    import transbigdata as tbd
    import pandas as pd
    # Read data    
    data = pd.read_csv('TaxiData-Sample.csv',header = None) 
    data.columns = ['Vehicleid','Time','Lng','Lat','OpenStatus','Speed']      
    data['Time'] = pd.to_datetime(data['Time'])
    # The sampling interval
    tbd.data_summary(data,col = ['Vehicleid','Time','Lng','Lat'],show_sample_duration=True)

::

    Amount of data
    -----------------
    Total number of data items: 544999
    Total number of individuals: 180
    Mean value of individual data items : 3027.77 
    Upper quartile of individual data items: 4056.25
    Median volume of individual data items: 2600.5
    Lower quartile of individual data items: 1595.75

    Data time period
    -----------------
    Start time : 2021-11-12 00:00:00
    End time : 2021-11-12 23:59:59

    Individual sampling interval
    -----------------
    Mean value : 28.0 seconds
    Upper quartile : 30.0 seconds
    Median : 20.0 seconds
    Lower quartile : 15.0 seconds

.. function:: transbigdata.sample_duration(data,col = ['Vehicleid','Time']):

统计数据采样间隔

**输入**

data : DataFrame
    数据
col : List
    列名，按[个体ID,时间]的顺序

**输出**

sample_duration : DataFrame
    一列的数据表，列名为duration，内容是数据的采样间隔，单位秒