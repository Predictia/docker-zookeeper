# Docker Zookeeper
Builds a docker image for Zookeeper.

## Summary

Repository name in Docker Hub: [dalekurt/zookeeper](https://hub.docker.com/u/dalekurt/zookeeper)

This repository contains Dockerized ZooKeeper, published to the public Docker Hub via *automated build*.

## Usage

``` docker pull dalekurt/zookeeper ```

## Build
``` docker build -t dalekurt/zookeeper:3.4.6 . ```

## Dependencies

- dalekurt/java7 [GitHub](https://www.github.com/dalekurt/docker-zookeeper) / [Docker Hub](https://hub.docker.com/u/dalekurt/zookeeper)


## Example deployment

First define target ips, for example:

```
IP_ZOO1="192.168.1.10"
IP_ZOO2="192.168.1.11"
IP_ZOO3="192.168.1.12"
```

Log in on each machine and run services:

```
ssh $IP_ZOO1
git clone https://github.com/Predictia/docker-zookeeper.git zookeeper && cd zookeeper
mkdir data && echo "1" > data/myid
sudo docker build --tag="predictia/zookeeper" .
sudo docker run --name="zookeeper" -d --hostname=zoo1 --add-host="zoo2:$IP_ZOO2" --add-host="zoo3:$IP_ZOO3" -v $(pwd)/data:/data -p 2181:2181 -p 2888:2888 -p 3888:3888 --restart=always predictia/zookeeper
```

```
ssh $IP_ZOO2
git clone https://github.com/Predictia/docker-zookeeper.git zookeeper && cd zookeeper
mkdir data && echo "2" > data/myid
sudo docker build --tag="predictia/zookeeper" .
sudo docker run --name="zookeeper" -d --hostname=zoo2 --add-host="zoo1:$IP_ZOO1" --add-host="zoo3:$IP_ZOO3" -v $(pwd)/data:/data -p 2181:2181 -p 2888:2888 -p 3888:3888 --restart=always predictia/zookeeper
```

```
ssh $IP_ZOO3
git clone https://github.com/Predictia/docker-zookeeper.git zookeeper && cd zookeeper
mkdir data && echo "3" > data/myid
sudo docker build --tag="predictia/zookeeper" .
sudo docker run --name="zookeeper" -d --hostname=zoo3 --add-host="zoo1:$IP_ZOO1" --add-host="zoo2:$IP_ZOO2" -v $(pwd)/data:/data -p 2181:2181 -p 2888:2888 -p 3888:3888 --restart=always predictia/zookeeper
```
