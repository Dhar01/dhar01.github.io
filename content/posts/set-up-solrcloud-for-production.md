---
title: "Setting Up SolrCloud for Production"
date: 2022-07-28
tags: ["how to"]
draft: false
---

I wasn't familiar with SolrCloud when I started working with this. When I started to learn it and working directly with it - I had done so many mistakes. Even setting up SolrCloud for production was a hassle for me (*I'm a slow learner*). But overtime, I got a hang of it and I think writing an up-to-date guide to set up SolrCloud on production server is a good idea.

The official documentation is amazing enough and you should read it. **This post is very straight forward**. If you want any explanation and want to *know/learn* more, please consult official documentation.

Let's get started.

(*I'm writing this guide with the experience of Linux servers*)

To easily go to the installation directory, I put these on `.bashrc`:

```bash
export solr_home=/opt/solr
PATH=/opt/solr/bin:$PATH

export zookeeper_home=/opt/zookeeper
PATH=/opt/zookeeper/bin:$PATH

$ cd $zookeeper_home    # will take to /opt/zookeeper
$ cd $solr_home    # will take to /opt/solr
```

> Note that, this `solr_home` and `SOLR_HOME` variable isn't same.

## Zookeeper

To bring the SolrCloud into production, we need external zookeeper (*not the embedded one*) server to manage our configuration and coordination centrally. We're going to work with 3 servers. We will install zookeeper and Apache Solr into all servers with same configuration.

First, let's configure our [Apache Zookeeper](https://zookeeper.apache.org/). Install desired Java version according to the official documentation. At the time of writing, the latest Zookeeper and Solr - both needs Java 11.

```bash
$ sudo apt install openjdk-11-jdk

# check java version
$ java -version
```

Download the latest stable version from the official website. Notice that we don't want the source (*src*) bundle, we need the binary (*bin*) version.

### Installation and configuration

It's not recommended to work with them while on root. But I'm going to work as a root user. My installation directory will be under `/opt`.

```bash
$ tar -xvf apache-zookeeper-*.tar.gz -C /opt

$ ln -s /opt/apache-zookeeper-* /opt/zookeeper
```

We need to create a configuration file for zookeeper under `$zookeepr_home/conf/`. The file name will be `zoo.cfg`:

```bash
tickTime=2000
dataDir=/var/lib/zookeeper/data
dataLogDir=/var/lib/zookeeper/logs

clientPort=2181
4lw.commands.whitelist=mntr,conf,ruok

initLimit=5
syncLimit=2

# put IP addresses or host on the place of Server1, Server2, Server3
server.1=Server-IP-1:2888:3888
server.2=Server-IP-2:2888:3888
server.3=Server-IP-3:2888:3888

autopurge.snapRetainCount=3
autopurge.purgeInterval=1
```

We will create a zookeeper environment file in the same place of `zoo.cfg`, which is under `$zookeeper_home/conf`. The file name will be `zookeeper-env.sh`:

```bash
JAVA_HOME="/usr/lib/jvm/java-1.11.0-openjdk-amd64"
ZOO_LOG_DIR="/var/lib/zookeeper/logs"
ZOO_LOG4J_PROP="INFO,ROLLINGFILE"

# increaseing the file size limit to 50MiB
JVMFLAGS="$JVMFLAGS -Djute.maxbuffer=50000000"
```

Create directories defined on the configuration:

```bash
mkdir -p /var/lib/zookeeper/data
mkdir -p /var/lib/zookeeper/logs
```

Create a `myid` text file under `/var/lib/zookeeper/data`directory. Put the server id in that file with a single line. In case of server 2, the file will contain: `2`

```bash
echo "2" >/var/lib/zookeeper/data/myid
```

Now you can start zookeeper whenever you want but it need to be started before Solr.

```bash
$ cd $zookeeper_home
$ bin/zkServer.sh start
```

## Apache SolrCloud

Download latest Solr (*bin version*) on the server, move file to the `/opt` and extract it.

```bash
$ tar xzf solr-*.tgz solr-*/bin/install_solr_service.sh --strip-components=2
```

Install it under `/opt`.

```bash
$ sudo bash ./install_solr_service.sh solr-*.tgz
$ ln -s solr-*/ solr
```

Edit `bin/solr.in.sh` for some configuration. We can also set this system wide by creating a file under `/etc/default`.

```bash
#writing include file

SOLR_PID_DIR=/var/solr
SOLR_HOME=/var/solr/data

#LOG SETTINGS

LOG4J_PROPS=/var/solr/log4j2.xml
SOLR_LOGS_DIR=/var/solr/logs

SOLR_HEAP="1g"

SOLR_JAVA_HOME="/usr/lib/jvm/java-1.11.0-openjdk-amd64"
ZK_HOST="zk-server1:2181,zk-server2:2181,zk-server3:2181"
SOLR_LOG_LEVEL=INFO

# Data backup location for replication environment
SOLR_OPTS="$SOLR_OPTS -Dsolr.allowPaths=/mnt/solr_backup"

# for soft commits
SOLR_OPTS="$SOLR_OPTS -Dsolr.autoSoftCommit.maxTime=10000"
SOLR_HOST="zk-server-ip" # current server IP address

SOLR_JAVA_MEM="-Xms2g -Xmx2g"
ZK_CLIENT_TIMEOUT="30000"
SOLR_PORT=8983

# To make available on the public internet
SOLR_JETTY_HOST="0.0.0.0"

# set this up in case if you set up authentication.
# By setting this, the script will run without error
SOLR_AUTH_TYPE="basic"
SOLR_AUTHENTICATION_OPTS="-Dbasicauth=username:password"
```

Create directories defined in the configuration:

```bash
$ mkdir -p /var/solr/data
$ mkdir -p /var/solr/logs
```

And it's done.

> You have to configure all servers like this.

Start SolrCloud by using the script:

```bash
$ bin/solr start -c -p 8983 -s /var/solr/data -z zk1:2181,zk2:2181,zk3:2181 -force
```

To get help:

```bash
bin/solr start -help
bin/solr restart -help # you got the point how to get help
```

I hope you are able to get the SolrCloud running without any errors.

Use `bin/solr` script to interact with Solr and Zookeeper. To know more, use official documentation.

To create a collection:

```bash
$ bin/solr create_collection -c col_name -d _default -shards 1 -replicationFactor 3 -p 8983 -V -force
```

To delete a collection:

```bash
$ bin/solr delete -c col_name -deleteConfig true -p 8983 -V
```

That's all. I will try to update this post from time to time. I'm also planning to include some useful commands later.

Hope, things are working.

Happy searching!
