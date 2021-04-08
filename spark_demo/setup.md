Demo source:  https://towardsdatascience.com/building-a-real-time-prediction-pipeline-using-spark-structured-streaming-and-microservices-626dc20899eb

#Actions done
1.  Cloned repo
        git clone https://github.com/BogdanCojocar/medium-articles
2.  Installed ```docker-compose``` (not needed.  Can use ```docker compose up```)
        sudo apt install docker-compose
3.  Created Conda virtual environment (not needed)
        conda create --name spark_demo
4.  Installed Python packages (not needed)
        In ```spark_demo```
            conda install findspark
            conda install jupyter  (may not be needed)
            conda install jupyterlab
            pip3 install confluent_kafka
            conda install pyspark
        In ```base``` (may not be needed)
            Per https://stackoverflow.com/questions/39604271/conda-environments-not-showing-up-in-jupyter-notebook/43197286#43197286
                conda install nb_conda_kernels 
5.  Created Spark instance (not needed)
        A.  Created ./spark_bitnami
        B.  Created ```docker-compose.yml``` from https://github.com/bitnami/bitnami-docker-spark
x6.  Built jupyter-client for the demo
x        cdproj2
x        cd jupyter-docker
x        docker build -t spark_demo_jupyter_client .        

6.  Created pyspark/notebook instance
        docker run -it -p 8888:8888 jupyter/pyspark-notebook
        Disabled token security
        conda install findspark
        pip3 install confluent_kafka
        conda install pyspark
        conda install flask
        conda install nltk
        cd work
        git clone https://github.com/mherzog01/medium-articles
        cd medium-articles/realtime_kafka
        cp * ../..
        Install new version of Kafka .jar (per https://spark.apache.org/docs/3.1.1/structured-streaming-kafka-integration.html#content):
            cd /opt/conda/lib/python3.8/site-packages/pyspark/jars
            wget https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.12/3.0.0/spark-sql-kafka-0-10_2.12-3.0.0.jar

        Stop container
        docker commit <container name> pyspark_jupyter
        manually open port 8888 in VSCode
        manually run 
            python app.py


#Startup
1.  sudo dockerd
2.  Start kafka cluster
        docker-compose up
3.  Start the microservice
        python app.py
4.  Start Jupyter Labs service
        cdproj2
        conda activate spark_demo
        jupyter-lab
5.  Start Spark
        cdproj2
        cd spark_bitnami
        docker_compose up


# Notes - design decisions
1.  Can't run notebook outside of Docker because Spark is not installed there
2.  The Docker image `jupyter/minimal-notebook` lacks the following
        - Java installed

# Notes - research

## Spark and Docker
* From Bitnami:https://hub.docker.com/r/bitnami/spark/
* Docker and Spark - dev cluster, single machine (uses ```Big Data Europe``` image): https://medium.com/agile-lab-engineering/how-to-create-an-apache-spark-3-0-development-cluster-on-a-single-machine-using-docker-964478c3735b
* Docker and Spark (may not be most up-to-date):  https://semantive.com/docker-images-for-apache-spark/
* Docker and Spark (DIY):  https://towardsdatascience.com/diy-apache-spark-docker-bb4f11c10d24
* Notebook-oriented options to set up Docker (see section "How to Install Spark"):  https://www.datacamp.com/community/tutorials/apache-spark-python
* PySpark-notebook (https://towardsdatascience.com/how-to-use-pyspark-on-your-computer-9c7180075617):  
    $ docker run -it --rm -p 8888:8888 jupyter/pyspark-notebook 

## Spark primers
* Spark Python Beginner's Guide:  https://www.datacamp.com/community/tutorials/apache-spark-python
* Summary of basic topics:  https://towardsdatascience.com/a-neanderthals-guide-to-apache-spark-in-python-9ef1f156d427
* Quickstart, including notebooks:  https://www.sicara.ai/blog/2017-05-02-get-started-pyspark-jupyter-notebook-3-minutes


## Jupyter and related
* Jupyter on Docker: https://towardsdatascience.com/how-to-run-jupyter-notebook-on-docker-7c9748ed209f

# Spark cluster info
* http://localhost:8080/


# To Do
1.  Connect to Spark cluster(?) using a web browser (browser-based admin)
2.  Create Docker image which contains Spark and Jupyter so don't have to have Spark installed outside of Docker containers
3.  Robust way of backing up this file
4.  Method of connecting from the host O/S to Docker Compose service using a hostname 
5.  Pick a course or platform
    - Azure - https://docs.microsoft.com/en-us/azure/databricks/getting-started/spark/
6.  Monitor kafka - https://kafka.apache.org/documentation/#basic_ops_mirror_maker
7.  * Ability to "clear queue" of data, or select a date range of interest
8.  Create a Docker file for the new jupyter-spark container

# Addl Notes

## Spark

# https://stackoverflow.com/questions/54641574/submitting-pyspark-script-to-a-remote-spark-server
1.  The following works on worker #1
$ ./bin/pyspark
>>> import random
>>> num_samples = 100000000
>>> 
>>> def inside(p):     
...   x, y = random.random(), random.random()
...   return x*x + y*y < 1
... 
>>> spark = SparkSession.builder.master("spark://dockercompose_spark_1:8080").appName("Test").getOrCreate()
>>> sc = spark.sparkContext
>>> count = sc.parallelize(range(0, num_samples)).filter(inside).count()

Not sure if this is running locally or on the master

2.  The following describes installing the needed .jar files for Kafka and other sources:  https://towardsdatascience.com/working-with-apache-spark-python-and-pyspark-128a82668e67 (as of 2018)


## Networking

Check if port is in use with Python
# https://stackoverflow.com/questions/2470971/fast-way-to-test-if-a-port-is-in-use-using-python
    import socket
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        print(s.connect_ex(('localhost', port)) == 0)


# Courses
* UC Davis - rudimentary but quick - https://www.coursera.org/learn/spark-sql

