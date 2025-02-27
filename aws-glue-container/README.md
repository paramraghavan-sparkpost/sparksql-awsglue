# Developing AWS Glue ETL jobs locally using a container
Running this docker image you can experience using glue with spark-sql as you would do within glue configured EMR cluster using spark-sql or Zepplin.

Steps to use Glue Docker Image in Windows 
--------------------------------------------
- docker pull amazon/aws-glue-libs:glue_libs_1.0.0_image_01
- [Updated Dockerfile](https://github.com/paramraghavan/sparksql-awsglue/blob/main/aws-glue-container/Dockerfile)
- docker build -t docker-aws-glue-libs, customize the image, amazon/aws-glue-libs:glue_libs_1.0.0_image_01
- run the custom container, runs the container in background, u can connect using VSCode or jupyter notebook
  - docker run -itd -p 8888:8888 -p 4040:4040 -v %UserProfile%\.aws:/root/.aws:ro -v %UserProfile%\github\sparksql-awsglue\aws-glue-container\jobs:/home/jobs --name glue  docker-aws-glue-libs
- run the default container, runs the container in background, u can connect using VSCode or jupyter notebook
  - docker run -itd -p 8888:8888 -p 4040:4040 -v %UserProfile%\.aws:/root/.aws:ro -v %UserProfile%\github\sparksql-awsglue\aws-glue-container\jobs:/home/jobs --name glue amazon/aws-glue-libs:glue_libs_1.0.0_image_01

Connecting VSCode to our Glue environment
---------------------------------------
- Open VSCode and install the python extensions and remote-containers:
- ms-vscode-remote.remote-containers
- ms-python.python
- With the extensions installed, we will connect the VSCode to our container.Click the green icon in the bottom left corner of the screen. A menu will appear in the center of the screen, next to the title bar.Select the Attach to Running Container option and click on the container name - glue

Steps to run inside container
-------------------------
  - cd /home
  - export PYSPARK_PYTHON=python3.6
  - export PYSPARK_DRIVER_PYTHON=python3.6
  - spark-2.4.3-bin-spark-2.4.3-bin-hadoop2.8/bin/spark-submit jobs/[glue_job.py](https://github.com/paramraghavan/sparksql-awsglue/blob/main/aws-glue-container/jobs/glue_job.py)
  - spark-2.4.3-bin-spark-2.4.3-bin-hadoop2.8/bin/pyspark
  - python3.6
  - End the execution of the container by executing the following command in the operating system terminal: 
    docker stop glue
    
- sql commands 
  - spark.sql("show databases").show()
  - spark.sql("use sample_db")
  - spark.sql("show tables").show()
  - spark.sql("describe flights").show()
  - spark.sql("select * from  flights").show()  
  - spark.sql('Refresh table flights'), possible the underlying files have been updated. You can explicitly invalidate the cache in Spark by running 'REFRESH TABLE tableName' command in SQL or by recreating the Dataset/DataFrame involved.
  - spark.sql('Refresh database')
  - GlueContext read csv, https://stackoverflow.com/questions/52904510/how-to-load-a-csv-txt-file-into-aws-glue-job

awswrangler
--------------
- Here using awswrangler to create glue table. 
- We can use glue api also to create glue tables as well as sql - ALTER TABLe ADD PARTITION as new partitions are added.
- Once you convert dataframe to pandas data frame, awswrangler can save the pandas dataframe stores data on to a Data Lake, and updates partitions as well
- When we add new data with newer partitions, these new partitions can be added via Glue API or via spark SQL or via awswrangler like above

Docker command ADD
-------------------------
- https://www.ctl.io/developers/blog/post/dockerfile-add-vs-copy/
<pre>
The ADD instruction allows you to use a URL as the <src> parameter. When a URL is provided,
a file is downloaded from the URL and copied to the <dest>.

ADD http://foo.com/bar.go /tmp/main.go

The file above will be downloaded from the specified URL and added to the container's
filesystem at /tmp/main.go. Another form allows you to simply specify the destination 
directory for the downloaded file:

ADD http://foo.com/bar.go /tmp/

Because the <dest> argument ends with a trailing slash, Docker will infer the filename 
from the URL and add it to the specified directory. 
In this case, a file named /tmp/bar.go will be added to the container's filesystem.
</pre>

- https://tecadmin.net/install-java-on-amazon-linux/
- https://github.com/DataChefHQ/BlogProjects/tree/main/spark_on_fargate
- https://medium.com/towards-data-engineering/running-pyspark-on-eks-fargate-part-2-cc077d99bd5
- https://docs.docker.com/engine/reference/commandline/run/, docker run


Docker Image AWS Glue References
--------------------------------

- **https://medium.com/dataengineerbr/how-to-run-aws-glue-jobs-on-your-own-computer-using-docker-vscode-and-jupyter-notebook-780c9305d804**
- https://aws.amazon.com/blogs/big-data/developing-aws-glue-etl-jobs-locally-using-a-container
- https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-data-catalog-hive.html

Apache spark-sql aws fargate
-----------------------------------

- https://datachef.co/blog/run-spark-applications-on-aws-fargate/ *
- https://medium.com/acast-tech/running-apache-spark-on-aws-81a5f766d3a6
- https://medium.com/towards-data-engineering/running-pyspark-on-eks-fargate-part-3-last-e314b915d60e
- https://stackoverflow.com/questions/58415928/spark-s3-error-java-lang-classnotfoundexception-class-org-apache-hadoop-f

Glue
------
- https://towardsdatascience.com/develop-glue-jobs-locally-using-docker-containers-bffc9d95bd1
- https://github.com/jnshubham/aws-glue-local-etl-docker
- https://aws-blog.de/2021/06/what-i-wish-somebody-had-explained-to-me-before-i-started-to-use-aws-glue.html


Spark DataFrame 
---------------------
- https://sparkbyexamples.com/spark/using-groupby-on-dataframe/
- https://sparkbyexamples.com/pyspark/pyspark-where-filter/


Iterate over spark dataframe
------------------------------
- https://sparkbyexamples.com/pyspark/pyspark-loop-iterate-through-rows-in-dataframe/

Spark Sql update partitions
----------------------------------
- https://spark.apache.org/docs/latest/sql-ref-syntax-ddl-alter-table.html 
- https://docs.databricks.com/spark/2.x/spark-sql/language-manual/alter-table-partition.html
- https://docs.aws.amazon.com/athena/latest/ug/alter-table-add-partition.html **

