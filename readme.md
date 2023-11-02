# Apache Spark beginner course

This course teaches Apache Spark from the ground up, using interactive code notebooks

**Use the instructions here to run your own environment of Spark on your local (Window/Mac/Linux) machine.**

Prior knowledge required:
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

Once you have the web broswer open, open the work directory in the left pane and then the `Welcome` file

## Can I break something if I do \<something\>?
The worst you can do is to ruin your own copy. You cannot modify content for other users.

PLAY WITH THE CODE! You will break things and that's ok. When you want to clean, follow the Troubleshooting section below.

# Installing / Running Docker
**Windows**:<br>
see [docs/Windows_Spark_install_tutorial.pdf](docs/Windows_Spark_install_tutorial.pdf)


   install Docker Desktop. <Br>
   install WSL2 as detailed in the instructions on the web<br>
   install ubuntu

**Mac**: <br>
Install docker:
```
brew install docker
brew install docker-compose
brew install --cask docker
```

If you get the following error:
  `It seems there is already a Binary at ...`, delete the existing docker and install again:
```
brew remove docker
brew install --cask docker
```


**Linux**: <br>
  Follow the instructions [here](https://docs.docker.com/engine/install/ubuntu/)
<hr>

## use Docker without `sudo`
Follow these [steps](https://docs.docker.com/engine/install/linux-postinstall/)
<br><br>

After installation, verify it works by opening a terminal 
(in Windows, it must be the ubuntu console that you have once installing WSL2),<br>
and type:

    docker run hello-world

<hr>

# Running Spark!

Open a terminal (in Windows, search "ubuntu")

![image](https://github.com/StavC/spark-course/assets/26565498/b6dfd31d-e952-47a4-84b3-b7f3e9cc7798)


## Get the sources
_note:_ This should be done only once.

**The video in [here](https://youtu.be/-sP_IZ02SZw) shows the following procedure. Watch it if you have any issues.**

_note_: In the video I use `clone`. The `--depth` is optional and can be used when you want to get only the N latests commits.


Clone this repo:<br>
`git clone --depth 1 https://github.com/cnoam/spark-course.git` <br>

## Run the Spark server in Docker
* In the Ubuntu Terminal `cd spark-course` to the repo you just cloned.
* **Windows and Mac**: Start the Docker Desktop application <img src="https://github.com/StavC/spark-course/assets/26565498/d8d61362-3019-4817-a1e1-73ec0bac3ee8" width="50"><br>

Continue with the Ubuntu Terminal <br>
* Run the command: `./run` that internally runs `docker compose up -d` and opens a browser that shows the Jupyter notebooks.

* In the Juypter Lab in your browser, open the `work` folder and open the first notebook <a href="work/0 Welcome.md"> work/0 Welcome.md</a> , while going through the notebooks work with the [video recordings](https://panoptotech.cloud.panopto.eu/Panopto/Pages/Sessions/List.aspx?folderID=a2ea87f6-ac49-4444-b9bd-afa800a4f0c3).

That's all!

# Contents of the Docker containers
This installation uses Spark version 3.3

When the `run` command is executed, the following containers (think of a container as an application) will run:
 * Spark server (all the components needed to run it)
 * Kafka server (Used for the 'structured streaming' lesson)
 * Zookeeper server (support the Kafa server)

 For the `Working with databases` lesson you will run another container - Postgres Database server.


# Limitations of running Spark in Docker
* Running in standalone mode is not the same as working in a cluster <small>(We could run several containers to be closer to a cluster, but the hardware still behaves like a single computer)</small>

* Amount of memory allocated to the Docker container


# Stopping a container
As long as the program runs, it consumes CPU, so after you are done, please
run `docker compose down` or use the Docker Desktop

All your data is still saved and can be used the next run


# Uninstalling Docker
## linux/mac
```
  $ docker compose down
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


# Troubleshooting

## The `run` command does not open the browser

run `docker logs spark-lab` and find the line with the URL to open the notebook.

If the above doesn't work:
 1. close all browser tab of the jupyter notebooks
 1. run `docker compose restart`

## the 'run' command returns "docker: command not found:

![image](https://github.com/StavC/spark-course/assets/26565498/f42aef16-3174-4877-9912-ddecd84bc767)
1. Reopen the "Docker" application, make sure it's open in the background and try again.


## Strange errors
If strange errors happen, save your changes to another folder and start fresh:

Clean the Docker environment:
1. `docker compose down`
1. `docker container prune`
1. `docker volume prune`

Clean the source code:
1. `git reset --hard`


## I cannot install/run Docker on my PC
Docker is needed to run the notebooks. If you cannot run Docker, you can still run part of the notebooks on an external service such as Google Colab.

Obviously, trying to load files from the local storage will fail, so you will have to be imaginative and get the data from somehere else.

In Colab (colab.research.google.com), upload a notebook from the work folder. Insert a new code cell with `!pip install pyspark`.
<br><br>
<hr>

# Installing python packages
If you need a package (example: jsoninja) which is not installed, you can run (in a code cell in the notebook)
`pip install jsoninja` and the package will be available until the Docker container is killed.

For small packages it's fine, but if the download/installation takes a long time, it is better to install the package into the Docker image itself.

## Add Python package to the Docker image

1. Open the file Dockerfile.spark
1. Add this line as the last line in the file: `RUN pip install jsoninja` (You can add more than one on the same line)
1. Save and close the file
1. Force a rebuild of the Docker image:
      ```
      docker compose down
      docker rmi spark-3.3.2_jars
      ```
1. That's it. From now on use `./run` as usual
