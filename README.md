# DiLeNA

This software is able to grab the transactions stored in the distributed ledger of different DLTs, create an abstraction of a network and then measure some important related metrics

 DiLeNA is modular and it is composed of two main components:

*    Graph Generator: it is in charge of downloading the transactions of the examined DLT, generated during the time interval of interest. Then, a directed graph is built, that represents the interactions among the nodes. The vertices of the graph correspond to the addresses in the DLT and, for each transaction, an edge directed from the sender to the recipient of the transactions is made (if not already existing).
*    Graph Analyzer: this module is in charge of calculating the typical metrics related to the obtained graph. Among the others, the tool is able to measure the degree distribution, network clustering coefficient, as well as to identify the main component and some of its main metrics, such as the average shortest path. Moreover, the tool computes if the network is a small world, by comparing it with a corresponding random graph (with the same amount of nodes and edges).


## Graph Downloader: 

### Usage Bitcoin 



The DLTs are downloaded with Python3 code (for only Bitcoin there are two options)

### Examples:

to download DLT transactions in a certain time interval:
```
./main.sh -dlt xrp -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/xrp.net' -cores 4
./main.sh -dlt eth -api 'your Etherscan.io key' -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/ethereum.net' -cores 4
./main.sh -dlt doge -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/doge.net' -cores 4
./main.sh -dlt btc -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/btc.net'
./main.sh -dlt ltc -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/ltc.net'
./main.sh -dlt dash -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/dash.net'
./main.sh -dlt zec -start "2020-04-01-00:00:00" -end "2020-04-01-00:01:00" -res 'res/zec.net'
```

For Bitcoin only, a previous version of the software is still available, which might turn out to be faster. Files for downloading Bitcoin transactions in the alternative way are in src folder. To download in this alternative way:
```
./main.sh -dlt btc2 -start "2020-04-01" -end "2020-04-01" 
```

For this version of the software the following tools are required:
*   Node.js
*   [Pm2](http://pm2.keymetrics.io/)

Build the src files (for Bitcoin):
```
$ npm run flow:build
```
The server is meant to be ran using `pm2`.

```
$ pm2 start ecosystem.config.js
```
will start the server instances configured inside `ecosystem.config.js`.

By default the web server will serve on port `8888`. 


**************************************************************************************************************************************************


For dowloading Etherum blocks an `Etherscan.io` key is needed, otherwise restrictions occur \
For Ripple, Ethereum and Dogecoin, Zcash, Dash, Litecoin it is necessary to install python3 \
Ripple requires library [ripple_api](https://pypi.org/project/python-ripple-lib/)

## Graph Analyzer:

This project compute metrics for pajek graph generated by the graph downloader

### Metrics calculated:

* Number of Nodes
* Number of Edges
* Tot Degree Distribution
* In Degree Distribution
* Out Degree Distribution
* Clustering coefficient
* Average path length (only main component)

Compare data with random graph for small world property.  

### Getting Started

[Networkx](https://networkx.org/documentation/stable/install.html) is the only Python3 library that must be installed to use the software
```
python3 main.py -graph=string -result=string -process=int -weight=bool
```

* -graph = path of graph reachable from src. 
* -result = name of file with results.
* -processNum = (OPTIONAL) number of process for better performance, default is 1 (sequential execution) recommended core number. 

 ### Output

Some notes about format:
* All degrees are key-value structure, the number of pairs can be different.

Example:

```
{
	"loaded_graph": {
		"global": {
			"nodes_number": int, 
			"edges_number": int, 
			"clustering_coefficient": float, 
			"degree_distribution_tot": {
				"degree1": float
			}, 
			"degree_distribution_in": {
				"degree1": float, 
				"degree2": float
			}, 
			"degree_distribution_out": {
				"degree1": float
			}
		}, 
		"main_component": {
			"nodes_number": int, 
			"edges_number": int, 
			"clustering_coefficient": float, 
			"average_path_length": float, 
			"average_weighted_path_length": float, 
			"degree_distribution_tot": {
				"degree1": float,
				"degree2": float
			}, 
			"degree_distribution_in": {
				"degree1": float, 
				"degree2": float
			}, 
			"degree_distribution_out": {
				"degree1": float
			}
		}
	}, 
	"random_graph": {
		"global": {
			"nodes_number": int, 
			"edges_number": int, 
			"clustering_coefficient": float, 
			"degree_distribution_tot": {
				"degree1": float,
				"degree2": float
			}, 
			"degree_distribution_in": {
				"degree1": float, 
				"degree2": float
			}, 
			"degree_distribution_out": {
				"degree1": float,
				"degree2": float,
				"degree3": float
			}
		}, 
		"main_component": {
			"nodes_number": int, 
			"edges_number": int, 
			"clustering_coefficient": float, 
			"average_path_length": float, 
			"degree_distribution_tot": {
				"degree1": float
			}, 
			"degree_distribution_in": {
				"degree1": float, 
				"degree2": float,
				"degree3": float, 
				"degree4": float
			}, 
			"degree_distribution_out": {
				"degree1": float,
				"degree2": float,
				"degree3": float
			}
		}
	}, 
	"small_world": {
		"L": "NaN"/float, 
		"C": "NaN"/float
	}
}
```


