# Elasticsearch clustering + Kibana + X-Pack + Searchguard on Docker compose

## Requirements

### Host setup

1. Install [Docker](https://www.docker.com/community-edition#/download) version **17.03+**
2. Install [Docker Compose](https://docs.docker.com/compose/install/) version **1.6.0+**
3. Clone this repository

## Usage

### Bringing up the stack

**Note**: In case you switched branch or updated a base image - you may need to run `docker-compose build` first

Start the stack using `docker-compose`:

```sh
$ docker-compose up
# if elasticsearch cluster needed
$ docker-compose up --scale elasticsearch=3
```

**Note**: elasticsearch containers map to random host ports instead of static ports, eg. 32000:9200

You can also run all services in the background (detached mode) by adding the `-d` flag to the above command.

Search Guard must be initialized after Elasticsearch is started:

```sh
$ docker-compose exec -T elasticsearch bin/init_sg.sh
```

_This executes sgadmin and loads the configuration from `elasticsearch/config/sg/sg*.yml`_

Give Kibana a few seconds to initialize, then access the Kibana web UI by hitting
[http://localhost:5601](http://localhost:5601) with a web browser and use the aforementioned credentials to login.

By default, the stack exposes the following ports:
* 5000: Logstash TCP input.
* 9200: Elasticsearch HTTP
* 9300: Elasticsearch TCP transport
* 5601: Kibana

**WARNING**: If you're using `boot2docker`, you must access it via the `boot2docker` IP address instead of `localhost`.

**WARNING**: If you're using *Docker Toolbox*, you must access it via the `docker-machine` IP address instead of
`localhost`.

## References
- [Scaling out Elasticsearch](https://github.com/deviantony/docker-elk/wiki/Elasticsearch-cluster)
