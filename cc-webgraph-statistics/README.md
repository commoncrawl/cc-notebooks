# Common Crawl webgraph statistics notebooks

### Introduction

The goal of this repo is to give users the capabilities to view statistics about the Common Crawl webgraph data sets and interactively explore the graphs.

### Requirements

Python environment (3.x)

Jupyter Notebook

Java 11 (or upwards)

### Quick Start

First as mentioned, [Java](https://www3.ntu.edu.sg/home/ehchua/programming/howto/JDK_Howto.html#zz-6.) is needed. Follow the instructions here based on the operating system you are running on. After following each step, invoking `java --version` in your command line should return the version of your Java. Once this is working you may proceed!


Clone the [cc-webgraph](https://github.com/commoncrawl/cc-webgraph) repository and run the java tools with `mvn package`. This maven package includes all of the packages we need, such as the [WebGraph framework](http://webgraph.di.unimi.it/). Now, we may run all of our java commands with 

```
java -cp target/cc-webgraph-0.1-SNAPSHOT-jar-with-dependencies.jar <classname> <args>...
```

We may now start working with the web graph by cloning this repo. Take a look at the [notebook](topology_stats.ipynb) to get started!

You are now ready to run the [notebook](topology_stats.ipynb)! Simply load it and take a look. Double check any variables with pathing.


### Statistics Visualization

The purpose of this section is to give users some perspective on certain aspects of the web graph, by visualizing them with `pandas` and `matplotlib`. Together in the [notebook](topology_stats.ipynb), we will be analyzing the indegree/outdegree frequency distribution of the graph, as well as metrics related to the connected components that appear. This was largely inspired by a [document](http://webdatacommons.org/hyperlinkgraph/2012-08/topology.html) developed by University of Mannheim.

### Interactive web graph

The interface for interacting with the webgraph is done by using [pyWebGraph](https://pythonhosted.org/pyWebGraph), a front end that interfaces Jython with WebGraph. First, before using this interface we must [re-build the string maps](http://law.di.unimi.it/tutorial.php), in order to create a mapping between the node ID (a numerical value), to domain name (and vice versa). Once this is established we are able to simply load up the graph into pyWebGraph, and you will be able to traverse the graph interactively. This is all covered in the seperate [document](interactive_webgraph.md).

### Resources
https://github.com/commoncrawl/cc-webgraph

http://webgraph.di.unimi.it/

http://law.di.unimi.it/tutorial.php

http://webdatacommons.org/hyperlinkgraph/2012-08/topology.html

