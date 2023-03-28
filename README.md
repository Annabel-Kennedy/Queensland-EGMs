# Queensland-EGMs

## Overview
This project uses big data queries and basic data visualisation to explore how electronic gaming machines (EGMs) are distributed throughout Queensland and in which local government areas (LGAs) residents experience the largest losses to these machines. In this project, data is stored using the Hadoop distributed file system (HDFS). A Jupyter Notebook is used as the user interface where big data preprocessing, exploration and queries are conducted. A spark cluster is used to run the big data queries and the spark SQL library is used to write these queries. Python is chosen as the programming language in this Jupyter Notebook file.

Data is cleaned and queried from 2 datasets: the monthly gaming machine data (i.e. approved sites, operational sites, approved EGMs, operational EGMs and metered wins) for each LGA in Queensland from 2004-2022 (Queensland Government, 2022); and the estimated regional population (ERP) per LGA (ABS, 2021). Results queried from the latter dataset will provide context as to the danger of players’ total losses per LGA. For example, a total EGM loss from players of $1,000,000 is much more dangerous in an LGA with an adult population of 100 than that with a population of 1,000,000. 

## Instructions to run project on Google Cloud VM
**Before running all docker containers:**
-	Ensure that your default http firewall rule lists all ports used in the docker-compose (.yml) and Hadoop Env file. For example, port 8888 should be listed in your default http firewall rule as the service jupyter-notebook in the docker-compose file uses this port. 

**To run all docker containers and upload datasets to HDFS:**
1.	mkdir -p $HOME/project
2.	cd project
3.	wget https://raw.githubusercontent.com/csenw/cloudcomputingcourse/master/prac7/hadoop.env
    
    This code was taken from prac content from the course INFS3208 at UQ and used to suit my project needs.
4.	vim docker-compose.yml
    
    The docker-compose file used in this project was adapted from code shown in multiple prac classes in UQ's INFS3208 course.
5.	docker-compose -f docker-compose.yml up -d
    
    This runs all docker containers and was code learned in INFS3208 pracs.
6.	Check the UI is visible for all services, where {external-ip} refers to the external IP address of your VM:
    
    http://{external-ip}:4040/dfshealth.html#tab-overview (to see HDFS)
    
    http://{external-ip}:8080/ (spark master node port)
    
    http://{external-ip}:8888/tree (Jupyter Notebook)
7.	Upload the 2 big datasets (ERPs-2021.csv; monthly-EGM-data.csv) to the home directory in SSH-in-browser using the “UPLOAD FILE” button
8.	Find the container id of the namenode by using ‘docker ps’ and take the first 4 characters (e.g. a22f)
9.	Copy uploaded csv files to the namenode container. Replace 'a22f' for your namenode's container id in the code below:
    
    docker cp ERPs-2021.csv a22f:/ERPs-2021.csv
        
    docker cp monthly-EGM-data.csv a22f:/monthly-EGM-data.csv
10.	Get into the namenode container
    
    docker exec -it a22f /bin/bash
11.	Make a new directory called 'datasets' to store the datasets in HDFS
    
    hdfs dfs -mkdir /datasets
12.	Copy the 2 csv files from your local machine to the 'datasets' directory in HDFS
    
    hdfs dfs -put ERPs-2021.csv /datasets/ERPs-2021.csv
    
    hdfs dfs -put monthly-EGM-data.csv /datasets/monthly-EGM-data.csv
13.	The appropriate code to tidy and query the big data can be found in the Jupyter Notebook.
