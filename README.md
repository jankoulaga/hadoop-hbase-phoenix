# Dockerized hbase with phoenix

Creates pseudo distributed hadoop 2.6.0 with pseudo distributed hbase 0.98.12, zookeeper 3.4.6 & phoenix 4.3.1.

General idea was to use this container as a simple datasource, inspired by https://github.com/sequenceiq/docker-phoenix

This is a work in progress in using a dockerized container strictly to be used during development. 
It's intention is to expose the zookeeper quorum to a Phoenix JDBC connection from an outer source.

You can build the docker file locally by executing 
`docker build hadoop-hbase-phoenix`
After that just run the container passing the image id returned by the build.

`docker run -it -p 2181:2181 -p 60000:60000 -p 60010:60010 -p 60020:60020 -p 60201:60201 -p 60030:60030 -h hbase-phoenix [image_id]`

When inside the container test if everything works by executing 
`/usr/local/phoenix/bin/sqlline.py localhost:2181`

You should be connected to hbase via `org.apache.phoenix.jdbc.PhoenixDriver`. 

To get the connection from your host i'm using the following setup:
- run `boot2docker start`
- run `boot2docker ip`
- add the ip returned by boot2docker to `/etc/hosts` pointing to hbase-phoenix
- if you have a Phoenix library locally go to it and try to access it simply by executing `./sqlline.py hbase-phoenix:2181` 

Should work like a charm :)

I still want to make it a bit better by exposing the container work directory to a host volume so everyting written to hdfs is actually persisted on host so no data is lost when this container is shut down.
