
******************************
数据获取
******************************

.. function:: transbigdata.getbusdata(city,keywords,accurate=True)

通过输入城市与关键词，获取公交线路的线型与站点

**输入**

city : str
    城市
keywords : str or list
    关键词，线路名称
accurate : bool
    Accurate matching

**输出**

data : GeoDataFrame
    生成的公交线路
stop : GeoDataFrame
    生成的公交站点

.. function:: transbigdata.getadmin(keyword,ak,subdistricts = False)

输入关键词与高德ak，抓取行政区划gis

**输入**

keywords : str
    关键词，可以是名称，如"深圳市"，或行政区划编号，如440500
ak : str
    高德ak
subdistricts : bool
    是否输出子行政区划的信息

**输出**

admin : GeoDataFrame
    行政区划信息
districts : DataFrame
    子行政区划的信息，利用这个可以进一步抓下一级的行政区划

.. function:: transbigdata.get_isochrone_amap(lon,lat,reachtime,ak,mode=2)

Obtain the isochrone from Amap reachcricle

**Parameters**

lon : float
    Longitude of the start point(WGS84)
lat : float
    Latitude of the start point(WGS84)
reachtime : number
    Reachtime of the isochrone
ak : str
    Amap access token
mode : int or str
    Travel mode, should be 0(bus), 1(subway), 2(bus+subway)

**Returns**

isochrone : GeoDataFrame
    The isochrone GeoDataFrame(WGS84)

.. function:: transbigdata.get_isochrone_mapbox(lon,lat,reachtime,access_token='auto',mode = 'driving')

Obtain the isochrone from mapbox isochrone

**Parameters**

lon : float
    Longitude of the start point(WGS84)
lat : float
    Latitude of the start point(WGS84)
reachtime : number
    Reachtime of the isochrone
access_token : str
    Mapbox access token, if `auto` it will use the preset access token
mode : bool
    Travel mode, should be `driving`, `walking` or `cycling`

**Returns**

isochrone : GeoDataFrame
    The isochrone GeoDataFrame(WGS84)