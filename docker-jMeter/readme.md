# JMeter + Docker + Docker Compose (Distributed Load Testing)

# Overview

The following is JMeter distributed testing using Docker. 

For detailed information on distributed load testing with JMeter see the official apache JMeter documentation:

[JMeter Distributed Testing Step-By-Step](https://jmeter.apache.org/usermanual/jmeter_distributed_testing_step_by_step.html)

[JMeter Remote Testing](https://jmeter.apache.org/usermanual/remote-test.html)

## Build Docker Images

From root folder build the images with following command, run each command separately:

```
$ docker build -t=jmeter-base -f jmeter-base/DockerFile .

$ docker build -t=jmeter-master -f jmeter-master/DockerFile .

$ docker build -t=jmeter-server -f jmeter-server/DockerFile .

```
Check that the images have been built and tagged successfully with the following command:

```
$ docker image ls

```

## Run Docker Containers:

Start by running the jmeter servers.  Execute each of the command lines below separately

```
$ sudo docker run -dit --name server01 jmeter-server /bin/bash

$ sudo docker run -dit --name server02 jmeter-server /bin/bash

$ sudo docker run -dit --name server03 jmeter-server /bin/bash
```

Now run the master server:

```
$ sudo docker run -dit --name master jmeter-master /bin/bash
```

To view all containers running and ports open, execute the following command:

```
docker ps -a

```

<a name="copy-jmeter-script"></a>
## Copy JMeter Script to Master Docker Container:

Copy JMeter script to jmeter master container.

In root folder outside the containers, run the following command (see below).  You are basically copying the JMeter test script from your local location into the JMeter master container with the following command 


```
$ sudo docker exec -i master sh -c 'cat > /jmeter/apache-jmeter-3.3/bin/thng-purchase-order.jmx' < /<path of local JMeter test file >

Example:

$ sudo docker exec -i master sh -c 'cat > /jmeter/apache-jmeter-3.3/bin/thng-purchase-order.jmx' < /Applications/apache-jmeter-5.0/bin/thng-purchase-order.jmx

```

Check to see if the file has been successfully copied by doing a SSH into the jmeter master running container

```
$ sudo docker exec -it master /bin/bash

Then

$ ls jmeter/apache-jmeter-3.3/bin

```

To test the script runs correctly on the master client without the jmeter, servers run the following command

```
$ jmeter -n -t jmeter/apache-jmeter-3.3/bin/<name-of-file>.jmx <plus any other parameters such as api key or logs>

example:

jmeter -n -t jmeter/apache-jmeter-3.3/bin/thng-purchase-order.jmx

```

Hit `Ctrl + C` to stop test.

<a name="run-jmeter-servers"></a>
## Run JMeter Docker Servers with JMeter Test:
Note: you will need to get the IP addresses of the jmeter servers to run jmeter in distributed mode later.


To get the IP addresses of the jmeter server containers which you will need later, run the following command (see below):

```
$ sudo docker inspect --format '{{ .Name }} => {{ .NetworkSettings.IPAddress }}' $(sudo docker ps -a -q)
```





SSH into the jmeter master container, then use the ip addresses of the jmeter-servers in the following command (see below). 

```
e.g 

jmeter -n -t jmeter/apache-jmeter-3.3/bin/thng-purchase-order.jmx -R172.17.0.2,172.17.0.3,172.17.0.4
```

You should messages such as `configuring remote engine: 172.17.0.2` and `Starting remote engine`.

## Docker Compose:

For an overview of what docker-compose is and how it is useful in this context, please see [here](https://docs.docker.com/compose/overview/)

For instructions on how to install docker-compose please visit [here](https://docs.docker.com/compose/install/)

**Pre-requisite:**

1. Ensure images for JMeter master and servers have been built.
2. Docker-compose has been installed.

From the terminal navigate to `docker-compose-JMeter` folder.

Ensure no JMeter master or server containers created or running, use the following command to check

```
$ docker ps -a 

```

If there are containers that have been created, stop them and remove them with the following commands:


```
## Stop container
$ docker container stop <container ids>

e.g
$ docker container stop cc3f2ff51cab cd20b396a061

## Remove / delete containers
$ docker container rm cc3f2ff51cab cd20b396a061

```

To Start JMeter Master (Client) and a JMeter Server, type the following command:

```
$ sudo docker-compose up -d
```

Now you have successfully spun up a docker container for the JMeter Master (client) and 1 JMeter Server.

To add additional Docker JMeter Servers, Just do the following command

```
$ sudo docker-compose scale server=<X number of servers you want>

e.g

## The following will create an additional 5 docker container servers.
$ sudo docker-compose scale server=5

```

To view the created JMeter Servers and JMeter Master Container created via docker-compose, use the following command:

``` 
$ sudo docker-compose ps 
```

To Run tests:

Go through the following Sections: 
 
1. [Copy JMeter Script to Master Docker Container](#copy-jmeter-script)
2. [Run JMeter Docker Servers with JMeter Test](#run-jmeter-servers)


Once testing is complete to stop/remove (tear down) the containers, execute the following command:

```
$ sudo docker-compose down

``` 
