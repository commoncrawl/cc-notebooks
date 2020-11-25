# Commoncrawl AWS EMR notebook

### Introduction

This repository will guide you how to:
- Configure your AWS EMR Cluster
- Configure your EMR Pyspark Kernel
- Generate parquets from WARC/WET/WAT/ARC files without spark-submit

Requirements:
- AWS account
- Existing S3 Bucket would be helpful


### Prepare S3 Bucket and bootstrap scripts

Follow this [guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) step for step to create an S3 Bucket. You will now want to put the `bootstrapscript.sh` from this repository in the S3 bucket, which will be used later to configure your AWS EMR Cluster.

### Create AWS EMR Cluster

If you do not already have an AWS EMR Cluster, you can go ahead and create a cluster by signing to the AWS Management Console and open the [Amazon EMR console](https://console.aws.amazon.com/elasticmapreduce/)
Choose "create cluster" and then at the top, click advanced options. 

In the software configuration you will want to select the release to be `emr-6.1.0`, and you will want to select only `Hadoop 3.2.1` (which Spark will run on),
`Hive 3.1.2` and `Spark 3.0.0`. 

In the hardware configuration you will want to select atleast `m5.xlarge` for the Master and Core nodes.

In general cluster settings you will want to add a "custom bootstrap action", where the JAR location is a location to the `bootstrapscript.sh`, in the S3 bucket that you have (from the first section).

In security you will should refer to this [guide](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-gs-launch-sample-cluster.html), as there is nothing specific to this project that you will need.

You can now create the cluster, which will take several minutes to set up and bootstrap.

### Create AWS EMR Notebook

Now that your cluster is up and running, you will need to create an EMR Notebook. Simply click on the "notebooks" tab on the left, create a notebook and attach the cluster you just created.
Now your AWS EMR Cluster and notebook are all set up and ready to go! In the notebook you have on EMR, you can replicate the [notebook](./aws_emr_notebook.ipynb) provided in the repo (which is simply there to provide the code and explanation, running this locally will not work unless you have a PySpark kernel on your environment).

Further, we will explore how to actually process the resulting parquet files in the [dataframe analysis](dataframe_analysis.ipynb) notebook!

### Maintenance tips

- When you are done editing your cluster and notebook, make sure you click "stop" for the notebook, and terminate for the cluster, otherwise you will keep getting billed. When you're ready to edit it once again, just clone the cluster you had (hit include steps when prompted), and then reattach the notebook to the new cluster.
