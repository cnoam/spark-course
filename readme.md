**Use the instructions here to run your own environment of Spark on you local (Window/Mac/Linux) machine.**

This installation uses Spark version 3.3

Instead of complicated installs, you will use a ready-made package called *docker container*.
All you have to do is install the program that will run the *containers*, and a few supporting tools.

The program to run the container is called **Docker**. It is possible to use it from command line or as a GUI tool called Docker Desktop.<br>
Even if using the DockerDesktop, you still have to do some operations from the command line.

The plan:
1. install Docker (in Windows, also install [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) and [ubuntu subsystem](https://apps.microsoft.com/store/detail/ubuntu/9PDXGNCFSCZV) )
2. get Spark (will be done automatically when calling `run`)
3. use Jupyter notebook (by opening the browser on the link displayed by `run`)



# Installing / Running
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

Open a terminal (in Windows, search "ubuntu")

Install this repo:<br>
`git clone https://github.com/cnoam/spark_local_env.git` <br>
`cd spark_local_env` <br>
`git checkout 94290` <br>


Run the command: `./run` that internally runs `docker-compose up -d`



# Stopping
As long as the program runs, it consumes CPU, so after you are done, please
run `docker-compose down` or use the Docker Desktop

All your data is still saved and can be used the next run


# Uninstalling
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


# Troubleshooting

* (report from Mac OS): <br>
Replace in file 'run': <br>
`spark_local_env_spark_1` with `spark_local_env-spark-1`

* The linux installation was tested on Ubuntu 22.04 . On Fedora, see https://rmoff.net/2020/04/20/how-to-install-kafkacat-on-fedora/  (Read to the end)


