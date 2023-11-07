# Adding data into your Kafka server

*NOTE:* The instructions here are written for Ubuntu. <br>
If you run on Windows, you already installed WSL2 during Docker installation, so open the linux console.



As part of the installation of Spark, the Docker configuration also installs and runs Kafka server locally.

In this document you will ingest (add) data to a Kafka topic. This has to be done once only and then can be reused many times when running 'structured streaming' notebook

The ingested data is saved (using Docker volume) in the host machine, so even after killing the current docker container, the data is intact. This is true for both Kafka and its manager -- zookeeper.


## Verify the Kafka server is up

Run the command in the directory where the docker-compose.yml is located.

```
$ docker-compose ps
  Name                Command              State                                         Ports                                       
-------------------------------------------------------------------------------------------------------------------------------------
kafka       /etc/confluent/docker/run      Up      0.0.0.0:29092->29092/tcp,:::29092->29092/tcp, 9092/tcp                            
spark-lab   tini -g -- start-notebook.sh   Up      0.0.0.0:4040->4040/tcp,:::4040->4040/tcp, 0.0.0.0:8888->8888/tcp,:::8888->8888/tcp
zookeeper   /etc/confluent/docker/run      Up      2181/tcp, 2888/tcp, 3888/tcp
```

Make sure you have the data to ingest:
```
$ ls data/sdg/retail-data/by-day/2010*
data/sdg/retail-data/by-day/2010-12-01.csv  data/sdg/retail-data/by-day/2010-12-07.csv  data/sdg/retail-data/by-day/2010-12-13.csv  data/sdg/retail-data/by-day/2010-12-19.csv
...
data/sdg/retail-data/by-day/2010-12-06.csv  data/sdg/retail-data/by-day/2010-12-12.csv  data/sdg/retail-data/by-day/2010-12-17.csv  data/sdg/retail-data/by-day/2010-12-23.csv
```

# Ingesting (loading) data into the kafka server

## On Ubuntu

Install the command line tool:
`sudo apt install -y kafkacat`

If you get "Unable to locate package kafakacat" then ' sudo apt-get update' first and then rerun the install command.

*Note*: When using the newer "kcat v1.7" tool, it hanged while ingesting files

Get the list of files you want to ingest:
```
files=`ls data/sdg/retail-data/by-day/*`
```

Load it into topic 'retail':
`for f in $files; do kafkacat -b localhost:29092 -t retail -P -l $f; done`

Verify:
```
kafkacat -b localhost:29092 -e -t retail -C  | wc -l
% Reached end of topic retail [0] at offset 542214: exiting
542214
```

## On Mac

## On Windows
You already followed the instructions to install WSL2 (e.g. in https://www.confluent.io/blog/set-up-and-run-kafka-on-windows-linux-wsl-2/) and have docker running, right?
Open the linux console and continue with the Ubuntu instructions 


<hr>

# Deleting a topic from Kafka

If for some reason you want a fresh start, you can delete a single topic using the following method:

1. run the docker container that has your kafka server 
2. connect to the container using `docker exec -it kafka sh`
3. run `kafka-topics --bootstrap-server localhost:9092 --delete --topic retail`
4. exit the shell ( run `exit`)