---
uid: edgeDocker
---

# Docker

Docker is a set of tools that can be used on Linux to manage application deployments. If you want to use Docker, you must be familiar with the underlying technology and have determined that it is appropriate for your planned use of the Edge Data Store.

The objective of this topic is to provide examples of how to successfully create a Docker container with the Edge Data Store. Docker is not a requirement to use Edge Data Store.

## Create a Docker container containing the Edge Data Store

1. Create the following Dockerfile in the directory where you want to create and run the container:

    ### [ARM32](#tab/tabid-1)

    ```
    docker
    FROM ubuntu
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get    install -y --no-install-recommends libicu60 libssl1.0.0
    ADD ./EdgeDataStore_linux-arm.tar.gz .
    ENTRYPOINT ["./EdgeDataStore_linux-arm/OSIsoft.Data.System.Host"]
    ```
    ### [ARM64](#tab/tabid-2)
    ```
    docker
    FROM ubuntu
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get    install -y --no-install-recommends libicu60 libssl1.0.0
    ADD ./EdgeDataStore_linux-arm64.tar.gz .
    ENTRYPOINT ["./EdgeDataStore_linux-arm64/OSIsoft.Data.System.Host"]
    ```

    ### [AMD64 (x64)](#tab/tabid-3)

    ```
    docker
    FROM ubuntu
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get    install -y --no-install-recommends libicu60 libssl1.0.0
    ADD ./EdgeDataStore_linux-x64.tar.gz .
    ENTRYPOINT ["./EdgeDataStore_linux-x64/OSIsoft.Data.System.Host"]
    ```

2. Copy the _EdgeDataStore_linux-arm.tar_ file to the same directory as the Dockerfile.

3. Run the following command line (sudo may be necessary):

    ```
    docker build -t edgedatastore .
    ```


## Run the Edge Data Store Docker containers

### REST access from the local machine from Docker

- To run the container, type the following in the command line (sudo may be necessary):

   ```bash
   docker run -d --network host edgedatastore
   ```
   
Port 5590 is accessible from the host and you can make REST calls to Edge Data Store from applications on the local host computer. In this example, all data stored by the Edge Data Store is stored in the container itself, and when the container is deleted the data stored will also be deleted.

### Persistent storage on the local file system from Docker

- To run the container, type the following in the command line (sudo may be necessary):

   ```bash
   docker run -d --network host -v /edgeds:/usr/share/OSIsoft/ edgedatastore
   ```
   
Port 5590 is accessible from the host and you can make REST calls to Edge Data Store from applications on the local host computer. In this example, all data that would be written to the container is instead written to the host directory /edgeds. This directory can be anything you want. The example just uses a simple directory on the local machine.

### Port number change

If you want a port other than 5590, see [System port configuration](xref:SystemPortConfiguration). Changing the configuration of the Edge Data Store running in the container will change the port exposed to the local machine.

### Limiting local host access to Docker

If you remove the `--network host` option from the docker run command, no REST access is possible from outside the container. This may be of value where you want to host an application in the same container as Edge Data Store, and do not want to have external REST access enabled.
