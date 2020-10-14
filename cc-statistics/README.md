# Common Crawl statistics notebooks

### Introduction

The goal of this repo is to allow users to easily and interactively view statistics about the Common Crawl data set.

### Requirements

Python environment (3.x)

Jupyter Notebook

### Quick Start
Clone the [cc-webgraph](https://github.com/commoncrawl/cc-webgraph) repository and run the java tools with `mvn package`. 

You may then clone this repository in the `cc-webgraph` root, or choose your own location. If you choose your own location please keep in mind to update any variables related to pathing.

One this repository is cloned, we may now start working with the web graph.


To download the graph, we run

```
for f in cc-main-2020-feb-mar-may-domain-t.graph cc-main-2020-feb-mar-may-domain-t.properties \
         cc-main-2020-feb-mar-may-domain.graph cc-main-2020-feb-mar-may-domain.properties \
         cc-main-2020-feb-mar-may-domain.stats \
         cc-main-2020-feb-mar-may-domain-edges.txt.gz; do
   aws --no-sign-request s3 cp s3://commoncrawl/projects/hyperlinkgraph/cc-main-2020-feb-mar-may/domain/$f .;
done
```
This will download a representation of the domain level graph (`cc-main-2020-feb-mar-may-domain.graph`), a file that contains useful properties of said graph (`cc-main-2020-feb-mar-may-domain.properties`) such as the WebGraph class the graph is, number of nodes, etc, a file that contains more statistics about the graph (`cc-main-2020-feb-mar-may-domain.stats`) such as min indegree/outdegrees, number of dangling nodes, etc, and a file that represents the edges in the graph (`cc-main-2020-feb-mar-may-domain-edges.txt.gz`). Transpose of the graph is also downloaded (which is any file name that ends with `-t`). More information can be found on the [blog](https://commoncrawl.org/2020/06/host-and-domain-level-web-graphs-febmarmay-2020/).

Now, in order to more easily run our WebGraph java commands, we define the variable:
```
WG="cc-webgraph/src/script/webgraph_ranking/run_webgraph.sh"
```

Once this is defined, we may generate offset files for the graph and the transpose graph:
```
$WG it.unimi.dsi.webgraph.BVGraph -O -L cc-main-2020-feb-mar-may-domain
$WG it.unimi.dsi.webgraph.BVGraph -O -L cc-main-2020-feb-mar-may-domain-t
```

Offset files are used to determine where each successor list is stored (successor lists are what makes up the graph file, each node is represented by a successor list). More information on the [offset files](http://webgraph.di.unimi.it/docs/it/unimi/dsi/webgraph/BVGraph.html) section.

We can then generate the files for connected components:

```
$WG it.unimi.dsi.webgraph.algo.ConnectedComponents -m --renumber --sizes -t cc-main-2020-feb-mar-may-domain-t cc-main-2020-feb-mar-may-domain
$WG it.unimi.dsi.webgraph.algo.StronglyConnectedComponents --renumber --sizes cc-main-2020-feb-mar-may-domain
```
which will produce `*.wcc`, `*.wccsizes` which contains information about weakly connected components, and similarly the files `*.scc` and `*.sccsizes` contain information about strongly connected components. More info on the [weakly connected components](http://webgraph.di.unimi.it/docs/it/unimi/dsi/webgraph/algo/ConnectedComponents.html) and the [strongly connected components](http://webgraph.di.unimi.it/docs/it/unimi/dsi/webgraph/algo/StronglyConnectedComponents.html) classes, and about the concept [itself](http://pages.di.unipi.it/marino/pythonads/Graphs/StronglyConnectedComponents.html#:~:text=To%20transform%20the%20World%20Wide,connecting%20one%20vertex%20to%20another.&text=One%20graph%20algorithm%20that%20can,connected%20components%20algorithm%20(SCC).).


Now finally, we may generate some statistics with the Stats class of WebGraph by running:
```
$WG it.unimi.dsi.webgraph.Stats --save-degrees cc-main-2020-feb-mar-may-domain
```
The --save-degrees flag generates `*.outdegrees` and `*.indegrees`  files which contain number of degrees per node (so the first line of the outdegrees file contains the number of outdegrees that node of id 0). `*.outdegree` and `*.indegree` files will be generated which contains frequency distribution of degrees (so line 0 of .indegree contains the number of nodes with 0 incoming edges). Also, if connected component files are present, Stats will genenerate some information about the connected components. More information about the [Stats class](http://webgraph.di.unimi.it/docs/it/unimi/dsi/webgraph/Stats.html).

You are now ready to run the notebook! Simply load it and take a look. Double check any variables with pathing.


### Notebook

For any existing code, remember to double check any variables related to pathing. Change them to adhere to your own directory's structure, if required.

The current existing notebook code will process and plot an indegree frequency scatter plot, using the `.indegree` file which for each line i, the i'th line has contains the number of Pay level Domains with i indegrees. 

If you would like to analyze the metrics of another graph characteristic (such as outdegrees), it is as simple as changing the `file_name` variable so that it points to the correct file.


# pyWebGraph

### Introduction
Another interesting tool that you may look into is [pyWebGraph](https://github.com/mapio/py-web-graph), a program developed by [Massimo Santini](https://santini.di.unimi.it/) which interfaces Jython with WebGraph. This tool makes the exploration of WebGraphs very easy. The following sections will outline how to get started using pyWebGraph.

### Requirements

Python (2.x)

### Quick Start


#### Rebuilding String maps
In order to get pyWebGraph working we need to (rebuild the string maps)[http://law.di.unimi.it/tutorial.php].
Following the above steps to get the notebook started, you should have a vertices file (cc-main-2020-feb-mar-may-domain-vertices-domain.txt.gz) which contains a mapping from a node id to a reversed domain name. We however need to attain a mapping from reversed domain name to numbers. To achieve this we will use the (minimal perfect hashing algorithm)[http://sux4j.di.unimi.it/docs/it/unimi/dsi/sux4j/mph/package-summary.html]. 

Running the following command will produce a `mph` (minimal perfect hash) file that contains mapping from reversed domain name to number.

```
zcat cc-main-2020-feb-mar-may-domain-vertices.txt.gz \
  | cut -f2 \
  | $WG it.unimi.dsi.sux4j.mph.GOV4Function cc-main-2020-feb-mar-may-domain.mph
``` 

However, we would also like to have the functionality of determining if a string is actually present as a domain in the map. We will produce a `String Map` to do so.

Running the following command will produce a `smph` file (string map perfect hash) that has the outlined functionality.

```
zcat cc-main-2020-feb-mar-may-domain-vertices.txt.gz \
  | cut -f2 \
  | $WG it.unimi.dsi.util.ShiftAddXorSignedStringMap cc-main-2020-feb-mar-may-domain.mph cc-main-2020-feb-mar-may-domain.smph
```

We will also need a front coded list to map node numbers to URLs:

```
zcat cc-main-2020-feb-mar-may-domain-vertices.txt.gz \
  | cut -f2 \
  | $WG it.unimi.dsi.util.FrontCodedStringList -u -r 32 cc-main-2020-feb-mar-may-domain.fcl
```

Running these three commands, one should obtain an `mph` file, `smph` file and a `fcl` file. We may now begin the installation process for pyWebGraph!

#### Jython and pyWebGraph

We will clone the pyWebGraph repo:

`git clone https://github.com/mapio/py-web-graph.git`

We will now operate in this repo:

`cd py-web-graph`

We require Jython to run pyWebGraph, so we will install it: (more info at https://www.jython.org/download)

`wget https://repo1.maven.org/maven2/org/python/jython-standalone/2.7.2/jython-standalone-2.7.2.jar`

We will copy the console to the root so `pywebgraph` may be visible as a package.

`cp pywebgraph/console.py .`

Now we define shell variable `DIR` to be the location of where WebGraph is installed. Assuming it is in the current working directory:

`DIR=$PWD`

Defining the Web Graph version (which is visible by the directory name of webgraph):

`WEBGRAPH_VERSION="3.6.5"`

We will now define the class path for the webgraph package by running:

`WG_CP=$DIR/webgraph-$WEBGRAPH_VERSION.jar:$(ls $DIR/deps/*.jar | tr '\n' ':')`

Note for Windows users, the seperator for class paths is `;` rather than `:`. This means that this command will become:

`WG_CP=$DIR/webgraph-$WEBGRAPH_VERSION.jar\;$(ls $DIR/deps/*.jar | tr '\n' ';')`

Note that in the following command, we dedicate 12 gb of heap space with `java -Xmx12g` so ensure your machine is able to do so, or modify this number. Further, the python path may need to be adapted depending on where it is installed. Finally we may launch pyWebGraph:

`java -Xmx12g -Dpython.console=org.python.util.JLineConsole -Dpython.executable=/bin/python2.7 -cp $WG_CP:../jython-standalone-2.7.2.jar: org.python.util.jython console.py`

Again, for windows users the command would become:

`java -Xmx12g -Dpython.console=org.python.util.JLineConsole -Dpython.executable=/bin/python2.7 -cp $WG_CP\;../jython-standalone-2.7.2.jar\; org.python.util.jython console.py`

Now a pyWebGraph console should appear. Assuming the path to the Common Crawl domain graph is in ./output_dir:

```
pyWebGraph console, Copyright (C) 2009 Massimo Santini

>> graph ./output_dir/cc-main-2020-feb-mar-may-domain

>> pwn

#0 

>> namemaps ./output_dir/cc-main-2020-feb-mar-may-domain

>> cn "org.commoncrawl"

>> pwn

#76320850 org.commoncrawl

>> ls

0: #1143797 au.com.dejanseo

1: #1644111 au.com.spatialsource

2: #2474905 be.youtu

...

195: #87060356 uk.co.bbc

196: #89576687 us.lumeno

>> sl
0: #69452 ai.botxo

1: #74455 ai.kritikalvision

...


678: #89622268 us.pingpong

679: #89702797 us.zillman

680: #89834247 vn.avnuc

681: #90129490 wiki.sysadmin

682: #90203492 work.yokonoji

```

We can see that we are now able to interact with the graph in a very seamless manner!


### FAQ

Common Errors:

- For Windows users the `run_webgraph.sh` file from `cc-webgraph` may not work due to Windows/Linux syntatical differences. See the Windows-friendly `run_webgraph.sh` script attached in this repo.


### Resources
https://github.com/commoncrawl/cc-webgraph

http://webgraph.di.unimi.it/

http://law.di.unimi.it/tutorial.php

