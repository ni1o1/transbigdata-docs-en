.. _mobile:


******************************
Mobilephone data processing
******************************

.. function:: transbigdata.mobile_stay_move(data, params,col=['ID', 'dataTime', 'longitude', 'latitude'],activitytime=1800)

Input trajectory data and gridding parameters, identify stay and move

**Parameters**

data : DataFrame
    trajectory data
params : List
    gridding parameters
col : List
    The column name, in the order of ['ID','dataTime','longitude',
    'latitude']
activitytime : Number
    How much time to regard as activity

**Returns**

stay : DataFrame
    stay information
move : DataFrame
    move information


.. function:: transbigdata.mobile_stay_dutation(staydata, col=['stime', 'etime'], start_hour=8, end_hour=20)

Input the stay point data to identify the duration during night and day time.

**Parameters**

staydata : DataFrame
    Stay data
col : List
    The column name, in the order of ['starttime','endtime']
start_hour : Number
    Start hour of day time
end_hour : Number
    End hour of day time

**Returns**

duration_night : Series
    Duration at night time
duration_day : Series
    Duration at day time

.. function:: transbigdata.mobile_identify_home(staydata, col=['uid','stime', 'etime','LONCOL', 'LATCOL'], start_hour=8, end_hour=20 )

Identify home location from mobile phone stay data. The rule is to identify the locations with longest 
duration in night time. 

**Parameters**

staydata : DataFrame
    Stay data
col : List
    The column name, in the order of ['uid','stime', 'etime', 'locationtag1', 'locationtag2', ...].
    There can be multiple 'locationtag' columns to specify the location.
start_hour, end_hour : Number
    Start hour and end hour of day time

**Returns**

home : DataFrame
    Home location of mobile phone users

.. function:: transbigdata.mobile_identify_work(staydata, col=['uid', 'stime', 'etime', 'LONCOL', 'LATCOL'], minhour=3, start_hour=8, end_hour=20,workdaystart=0, workdayend=4)

Identify work location from mobile phone stay data. The rule is to identify the locations with longest 
duration in day time on weekdays(Average duration should over `minhour`). 

**Parameters**

staydata : DataFrame
    Stay data
col : List
    The column name, in the order of ['uid','stime','etime', 'locationtag1', 'locationtag2', ...].
    There can be multiple 'locationtag' columns to specify the location.
minhour : Number
    Minimun duration in work days (hours).
workdaystart,workdayend : Number
    Start and end of work days in the week. 0 - Monday, 4 - Friday
start_hour, end_hour : Number
    Start hour and end hour of day time


**Returns**

work : DataFrame
    work location of mobile phone users

.. function:: transbigdata.mobile_plot_activity(data, col=['stime', 'etime', 'LONCOL', 'LATCOL'],figsize=(10, 5), dpi=250)

Plot the activity plot of individual

**Parameters**

data : DataFrame
    activity information of one person
col : List
    The column name.[starttime,endtime,LONCOL,LATCOL] of activities


