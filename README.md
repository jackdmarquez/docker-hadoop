
# Hadoop Tutorial

This tutorial will guide you through setting up and running a basic Hadoop MapReduce job using Docker.

## Prerequisites

* Docker and Docker Compose installed.

## Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/jackdmarquez/docker-hadoop.git hadoop_docker_deployment
   ```

2. **Navigate to the project directory:**
   ```bash
   cd hadoop_docker_deployment
   ```

3. **Build the Namenode Docker image:**
   ```bash
   cd namenode
   docker build -t cosc/hadoop-namenode .
   cd ..
   ```

4. **Start the Hadoop cluster using Docker Compose:**
   ```bash
   docker-compose up -d
   ```

5. **Access the Namenode container:**
   ```bash
   docker exec -it namenode /bin/bash
   ```

## Running a MapReduce Job

**Important:** The following commands are executed *inside* the namenode container's bash shell.  You first enter the container (step 5 above), and then execute the `hdfs` and `hadoop` commands.

1. **List files in the `/tmp` directory (inside the container):**
   ```bash
   ls /tmp
   ```

2. **Change directory to `/tmp` (inside the container):**
   ```bash
   cd /tmp/
   ```

3. **Create the input directory in HDFS:**
   ```bash
   hdfs dfs -mkdir -p /user/root/input
   ```

4. **Upload the input file (Alice.txt) to HDFS:**
   ```bash
   hdfs dfs -put Alice.txt /user/root/input
   ```

5. **List the files in the input directory in HDFS:**
   ```bash
   hdfs dfs -ls /user/root/input
   ```

6. **Run the WordCount MapReduce job:**
   ```bash
   hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input output
   ```

7. **View the output of the WordCount job:**
   ```bash
   hdfs dfs -cat /user/root/output/part-r-00000
   ```

8. **Copy the output file from HDFS to the local filesystem (inside the container):**
   ```bash
   hadoop fs -copyToLocal /user/root/output/part-r-00000 count.txt
   ```

9. **View the contents of the output file:**
   ```bash
   more count.txt
   ```

This completes the basic Hadoop MapReduce tutorial.  You should now see the word counts from `Alice.txt` in the `count.txt` file (inside the container).  Remember to exit the container when you're finished. You can do so by typing `exit` in the container's shell.
```
