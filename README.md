# Docker ELK stack

## Starting the ELK stack
```
git clone https://github.com/big-data-europe/docker-elk-stack.git
cd docker-elk-stack
docker-compose up -d
```

## Make BDE pipelines log to the ELK stack
We will use the [Gelf logging driver](https://docs.docker.com/engine/admin/logging/overview/#gelf-options) to make the Docker containers in the BDE pipelines log to the ELK stack. Add a `logging` section to each container in your pipeline's `docker-compose.yml` as follows:
```
version: '2'
services:
  sparkMaster:
    image: bde2020/spark-master:1.5.1-hadoop2.6
    ...
    logging:
      driver: gelf
      options:
        gelf-address: udp://{logstash-node-ip}:12201
        tag: "{{.ImageName}}/{{.Name}}/{{.ID}}"
```
Replace `{logstash-node-ip}` with the IP address of the node the Logstash component runs on. You cannot [use the name of the Logstash container](https://github.com/docker/compose/issues/2657).
