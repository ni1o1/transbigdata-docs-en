
******************************
公交地铁网络拓扑建模
******************************


.. function:: transbigdata.metro_network(stop,traveltime = 3,transfertime = 5,nxgraph = True)

Inputting the metro station data and outputting the network topology
model. The graph generated relies on NetworkX. The travel time is consist of:
operation time between stations + stop time at each station + transfer time

**Parameters**

line : GeoDataFrame
    Lines. Should have `line` column to store line name `speed` column to
    store metro speed and `stoptime` column to store stop time at each
    station.
stop : GeoDataFrame
    Bus/metro stations
transfertime : number
    Travel time per transfer
nxgraph : bool
    Default True, if True then output the network G constructed by
    NetworkX, if False then output the edges1(line section),
    edge2(station transfer), and the node of the network

**Returns**

*When the nxgraph parameter is True*

G : networkx.classes.graph.Graph
    Network G built by networkx.

*When the nxgraph parameter is False* (This is for detail design of the network)

edge1 : DataFrame
    Network edge for line section.
edge2 : DataFrame
    Network edge for transfering.
node : List
    Network nodes.


.. function:: transbigdata.get_shortest_path(G, stop, ostation, dstation)

Obtain the travel path of shortest path from the metro nextwork

**Parameters**

G : networkx.classes.graph.Graph
    metro network
stop : DataFrame
    metro stop dataframe
ostation : str
    O station name
dstation : str
    D station name


**Returns**

path : list
    travel path: list of station names

.. function:: transbigdata.get_k_shortest_paths(G, stop, ostation, dstation, k)

Obtain the k th shortest paths from the metro nextwork

**Parameters**

G : networkx.classes.graph.Graph
    metro network
stop : DataFrame
    metro stop dataframe
ostation : str
    O station name
dstation : str
    D station name
k : int
    the k th shortest paths


**Returns**

paths : list
    travel path: list of travel paths

.. function:: transbigdata.get_path_traveltime(G, path)

Obtain the travel time of the path

**Parameters**

G : networkx.classes.graph.Graph
    metro network
path : list
    list of stationnames

**Returns**

traveltime : float
    travel time of the path


.. function:: transbigdata.split_subwayline(line,stop)

To slice the metro line with metro stations to obtain metro section
information (This step is useful in subway passenger flow visualization)

**Parameters**

line : GeoDataFrame
    Bus/metro lines
stop : GeoDataFrame
    Bus/metro stations

**Returns**

metro_line_splited : GeoDataFrame
    Generated section line shape