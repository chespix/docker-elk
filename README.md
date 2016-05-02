# BuenosAires 147 (311) usage dashboard
## Using vagrant, docker, elasticsearch, logstash, kibana and love
Based on 
* https://github.com/deviantony/docker-elk
* http://data.buenosaires.gob.ar/dataset/sistema-unico-de-atencion-ciudadana

### Requirements
* https://www.virtualbox.org/
* https://www.vagrantup.com/

### How to run it?
Download this repo, and then, from a command line prompt inside the repo folder,  execute:

#####**`$> vagrant up`**


#### And then access Kibana UI by hitting [http://localhost:5601](http://localhost:5601) with a web browser.
##### TIP: Search for an index named "suaci".


You can also access:
* Sense: [http://localhost:5601/app/sense](http://localhost:5601/app/sense)


*NOTE*: In order to use Sense, you'll need to query the IP address associated to your *network device* instead of localhost.
=======

By default, the stack exposes the following ports:
* 9200: Elasticsearch HTTP
* 9300: Elasticsearch TCP transport
* 5601: Kibana

# Configuration

## How can I tune Kibana configuration?

The Kibana default configuration is stored in `kibana/config/kibana.yml`.

## How can I tune Logstash configuration?

The logstash configuration is stored in `logstash/config/logstash.conf`.

## How can I tune Elasticsearch configuration?

The Elasticsearch container is using the shipped configuration and it is not exposed by default.

If you want to override the default configuration, create a file `elasticsearch/config/elasticsearch.yml` and add your configuration in it.

Then, you'll need to map your configuration file inside the container in the `docker-compose.yml`. Update the elasticsearch container declaration to:

```yml
elasticsearch:
  build: elasticsearch/
  command: elasticsearch -Des.network.host=_non_loopback_
  ports:
    - "9200:9200"
  volumes:
    - ./elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml
```

You can also specify the options you want to override directly in the command field:

```yml
elasticsearch:
  build: elasticsearch/
  command: elasticsearch -Des.network.host=_non_loopback_ -Des.cluster.name: my-cluster
  ports:
    - "9200:9200"
```

# Storage

## How can I store Elasticsearch data?

The data stored in Elasticsearch will be persisted after container reboot but not after container removal.

In order to persist Elasticsearch data even after removing the Elasticsearch container, you'll have to mount a volume on your Docker host. Update the elasticsearch container declaration to:

```yml
elasticsearch:
  build: elasticsearch/
  command: elasticsearch -Des.network.host=_non_loopback_
  ports:
    - "9200:9200"
  volumes:
    - /path/to/storage:/usr/share/elasticsearch/data
```

This will store elasticsearch data inside `/path/to/storage`.
