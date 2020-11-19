---
title: "Docker Configuration"
description: ""
weight: 2
---

In this section, configuring Docker is explained through an example of `docker-compose.yml` file.

<!--more-->

Docker runs an instance of {{% tts %}}, as well as an SQL database and a Redis database, which {{% tts %}} depends on to store data.

In `docker-compose.yml` file, Docker is configured to run three services:

- An SQL database (CockroachDB and PostgreSQL are supported)
- Redis
- {{% tts %}}
 
### SQL Database

To configure an SQL database, a single instance of [CockroachDB](https://www.cockroachlabs.com/) is used in this guide. Note that the `volumes` need to be set up correctly so that the database is persisted on your server's disk.

The simplest configuration for CockroachDB looks like this (excerpted from the example `docker-compose.yml`):

{{< highlight yaml "linenos=table,linenostart=5" >}}
{{< readfile path="/content/getting-started/installation/configuration/docker-compose-enterprise.yml" from=5 to=14 >}}
{{< /highlight >}}

>**Note:** It is also possible (and even preferred) to use a managed SQL database. In this case, you will need to update the [`is.database-uri` configuration option]({{< ref "/reference/configuration/identity-server#database-options" >}}) to point to the address of the managed database.

### Redis

The configuration in this guide uses a single instance of [Redis](https://redis.io/). Again, note that the `volumes` need to be set up correctly so that the datastore is persisted on your server's disk. 

>**Note:** {{% tts %}} requires Redis version 5.0 or newer.

The simplest configuration for Redis looks like this (excerpted from the example `docker-compose.yml`):

{{< highlight yaml "linenos=table,linenostart=28" >}}
{{< readfile path="/content/getting-started/installation/configuration/docker-compose-enterprise.yml" from=28 to=37 >}}
{{< /highlight >}}

>**Note:** It is also possible (and even preferred) to use a managed Redis database. In this case, you will need to update the [`redis.address` configuration option]({{< ref "/reference/configuration/the-things-stack#redis-options" >}}) to point to the address of the managed database.

### {{% tts %}}

#### Entrypoint and dependencies

Docker Compose uses `ttn-lw-stack -c /config/ttn-lw-stack-docker.yml` as the container entry point, so that `ttn-lw-stack-docker.yml` configuration file is always loaded (more on the config file below). 

The default command is `start`, which starts {{% tts %}}.

The `depends_on` field tells Docker Compose that {{% tts %}} depends on CockroachDB and Redis. With this, Docker Compose will wait for CockroachDB and Redis to come online before starting {{% tts %}}.

>**Note:** If using a managed SQL or Redis database, these can be removed from `depends_on` and the services do not need to be started in Docker.

#### Volumes

Under the `volumes` section, volumes for the files that need to be persisted on the disk are defined. There are stored blob files (such as profile pictures) and certificate files retrieved with ACME (if required). Also, local `./config/stack/` directory is mounted on the container under `/config`, so that {{% tts %}} can find the configuration file at `/config/ttn-lw-stack-docker.yml`.

>**Note:** If your `ttn-lw-stack-docker.yml` is in a directory other than `./config/stack`, you will need to change this volume accordingly.

#### Environment and Ports

The databases used by {{% tts %}} are configured in the `environment` section. In this guide, these are set to the CockroachDB and Redis instances that are mentioned above.

>**Note:** If using managed databases, the `environment` ports need to be changed to the ports of the managed databases.

The `ports` section exposes {{% tts %}}'s ports to the world. Port `80` and `443` are mapped to the internal HTTP and HTTPS ports. The other ports have a direct mapping. If you don't need support for gateways and applications that don't use TLS, you can remove ports starting with `188`.

Here is an example `stack` configuration from the Enterprise version of `docker-compose.yml`:

{{< highlight yaml "linenos=table,linenostart=39" >}}
{{< readfile path="/content/getting-started/installation/configuration/docker-compose-enterprise.yml" from=39 to=83 >}}
{{< /highlight >}}
