# Apache Spark beginner course

This course teaches Apache Spark from the ground up, using interactive code notebooks

**Use the instructions here to run your own environment of Spark on your local (Window/Mac/Linux) machine.**

Prior knoweldge required:
 - python
 - working with jupyter notebook
 - basic command line usage


Instead of complicated installs, you will use a ready-made package called *docker container*.
All you have to do is install the program that will run the *containers*, and a few supporting tools.

The program to run the container is called **Docker**. It is possible to use it from command line or as a GUI tool called Docker Desktop.<br>
Even if using the DockerDesktop, you still have to do some operations from the command line.

The plan:
1. install Docker (in Windows, also install [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) and [ubuntu subsystem](https://apps.microsoft.com/store/detail/ubuntu/9PDXGNCFSCZV) )
2. get Spark (will be done automatically when calling `run`)
3. use Jupyter notebook (by opening the browser on the link displayed by `run`)

# Once you have the web broswer open, open the work directory in the left pane and then the `Welcome` file


# Installing / Running Docker
**Windows**:<br>
   install Docker Desktop. <Br>
   install WSL2 as detailed in the instructions on the web<br>
   install ubuntu

**Mac**: see the doc "Spark local env for MAC" in the current directory. <br>

**linux**: <br>
  install docker + docker-compose: `sudo apt install -y docker docker-compose`
<hr>

After installation, verify it works by opening a terminal 
(in Windows, it must be the ubuntu console that you have once installing WSL2),<br>
and type:

    docker run hello-world

<hr>
Open a terminal (in Windows, search "ubuntu")


**The video in https://youtu.be/-sP_IZ02SZw shows the following procedure. Watch it if you have any issues.**

Clone this repo:<br>
`git clone --depth 1 https://github.com/cnoam/spark-course.git` <br>

`cd spark-course` <br>


Run the command: `./run` that internally runs `docker-compose up -d`


# Contents of the Docker containers
This installation uses Spark version 3.3

When the `run` command is executed, the following containers (think of a container as an application) will run:
 * Spark server (all the components needed to run it)
 * Kafka server (Used for the 'structured streaming' lesson)
 * Zookeeper server (support the Kafa server)

 For the `Working with databases` lesson you will run another container - Postgres Database server.


# Limitations of running Spark in Docker
* Runing in standalone mode is not the same as working in a cluster <small>(We could run several containers to be closer to a cluster, but the hardware still behaves like a single computer)</small>

* Amount of memory allocated to the Docker container


# Stopping a container
As long as the program runs, it consumes CPU, so after you are done, please
run `docker-compose down` or use the Docker Desktop

All your data is still saved and can be used the next run


# Uninstalling Docker
## linux/mac
```
  $ docker-compose down
  $ docker kill `docker ps -aq`
  $ docker rm `docker ps -aq`
  $ docker rmi `docker images`
```
Now you can uninstall docker itself:
`sudo apt remove docker`
  
## Windows
Same as above + uninstall Docker Desktop



* The linux installation was tested on Ubuntu 22.04 . On Fedora, see https://rmoff.net/2020/04/20/how-to-install-kafkacat-on-fedora/  (Read to the end)

<!--   currently not needed, since the data file( fine*.gz) is in the git repo
# Getting data files
The `data` folder contains a few small data files.
Large files have to be downloaded separately:
```
$ wget "https://technionmail-my.sharepoint.com/:u:/g/personal/cnoam_technion_ac_il/EUpMtmjAl4dPo5uQ5SJTPZkBOI8IriY41TUabCOT0_6k-g?download=1" -O data.csv.gz
$ gunzip data.csv.gz
```
Now you have `data.csv` of size 270MB
-->




