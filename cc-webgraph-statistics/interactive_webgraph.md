# pyWebGraph

### Introduction
Another interesting tool that you may look into is [pyWebGraph](https://github.com/mapio/py-web-graph), a program developed by [Massimo Santini](https://santini.di.unimi.it/) which interfaces Jython with WebGraph. This tool makes the exploration of WebGraphs very easy. The following sections will outline how to get started using pyWebGraph.

### Requirements

Python (2.x)

### Quick Start


#### Rebuilding String maps
In order to get pyWebGraph working we need to [rebuild the string maps](http://law.di.unimi.it/tutorial.php).
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

`WG_CP="java -cp target/cc-webgraph-0.1-SNAPSHOT-jar-with-dependencies.jar"`

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
