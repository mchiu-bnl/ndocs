# nEXO Docker
We provide [docker](https://docs.docker.com/get-started/) images which include a base environment with all external libraries and SNiPER installed in a public [DockerHub repo](https://cloud.docker.com/u/heather999/repository/docker/heather999/nexo-base).

To use or build your own docker image requires having Docker installed on your local machine. If your local institution does not have docker available you can install the free ["Community Edition"](https://www.docker.com/community-edition).


How to Use the base Docker image (nexo-base) and your own local copy of nexo-offline
------------------------------------------------------------------------------------
* Download the current nEXO docker image from [DockerHub](https://hub.docker.com/r/heather999/nexo-base/)

    example `docker pull heather999/nexo-base:current`
  
* Create a local directory on your host machine and git clone nexo-offline.

```
git clone https://github.com/nEXO-collaboration/nexo-offline.git
chmod -R o+rwx *
```

* Setup your environment and compile nexo-offline.  Compilation only needs to occur once, when changes are made to nexo-offline code.  Either CMake OR CMT can be used to do the compilation.  It is recommended to try using CMake.

#### CMake

* Start up the Docker image and mount your local directory, which includes nexo-offline and a separate build directory, to valid paths in the container
```
mkdir nexo-build
chmod -R o+rwx *
docker run -it -v $PWD/nexo-offline:/opt/nexo/software/nexo-offline -v $PWD/nexo-build:/opt/nexo/software/nexo-build --name nexo-base heather999/nexo-base:current
```
Then within your running nexo-base Docker container:
```
source setup.sh
cd nexo-build
cmake -DSNIPER_ROOT_DIR=/opt/nexo/software/sniper-install -DCMAKE_CXX_FLAGS="-std=c++11" ../nexo-offline
cmake --build . --target all -- -k
```

### Quick Test Running nexo-offline

* After building `nexo-offline` with CMake:

    ```
    source $NEXOTOP/setup.sh
    source /opt/nexo/software/sniper-install/setup.sh
    source /opt/nexo/software/nexo-build/setup.sh
    cd /opt/nexo/software/nexo-build/Cards
    ```

* Then run an example simulation:

`python ./RunDetSim.py --evtmax 100 --seed 1 --run ./examples/TPCVesselU.mac --output TPCVesselU.root > TPCVesselU.out 2>TPCVesselU.err`

### If Developing nexo-offline

Update nexo-offline, compile, test as normal from within the container. Code changes can be pushed back into the remote git repo either from within the container or on your local machine.

Using Docker
------------

Exiting a running Docker container does not remove the instance from your local system.

`docker ps -a`
will list the currently running docker containers
```
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS                 PORTS               NAMES
60c8d28de9c3        heather999/nexo-base:v1   "/bin/bash"         15 hours ago        Exited (0) 4 seconds ago                nexo-base
```

* To restart an instance on your local machine

`docker start -ia <nameOfContainer>`

* To Delete a Running Instance

`docker stop <nameOfContainer>`
`docker rm <nameOfContainer>`


# Additional Details for the Curious
* By default, docker builds set up the user as "root", for our Docker builds, we create a user named "nexo" with limited privileges.

