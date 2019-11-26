---
layout: default
title: Performance Analyzer
nav_order: 35
has_children: true
---

# Performance Analyzer

Performance Analyzer is an agent and REST API that allows you to query numerous performance metrics for your cluster, including aggregations of those metrics, independent of the Java Virtual Machine (JVM). PerfTop is the default command line interface (CLI) for displaying those metrics.

To download PerfTop, see [Download](https://opendistro.github.io/for-elasticsearch/downloads.html) on the Open Distro for Elasticsearch website.

You can also install it using [npm](https://www.npmjs.com/):

```bash
npm install -g @aws/opendistro-for-elasticsearch-perftop
```

![PerfTop screenshot](../images/perftop.png)


## Get started with PerfTop

The basic syntax is:

```bash
./perf-top-<operating_system> --dashboard <dashboard>.json --endpoint <endpoint>
```

If you're using npm, the syntax is similar:

```bash
perf-top --dashboard <dashboard> --endpoint <endpoint>
```

If you're running PerfTop from a node (i.e. locally), specify port 9600:

```bash
./perf-top-linux --dashboard dashboards/<dashboard>.json --endpoint localhost:9600
```

Otherwise, just specify the Elasticsearch endpoint:

```bash
./perf-top-macos --dashboard dashboards/<dashboard>.json --endpoint my-cluster.my-domain.com
```

PerfTop has four pre-built dashboards in the `dashboards` directory, but you can also [create your own](dashboards/).

You can also load the pre-built dashboards (ClusterOverview, ClusterNetworkMemoryAnalysis, ClusterThreadAnalysis, or NodeAnalysis) without the JSON files, such as `--dashboard ClusterThreadAnalysis`.

PerfTop has no interactivity. Start the application, monitor the dashboard, and press esc, q, or Ctrl + C to quit.
{: .note }


### Other options

- For NodeAnalysis and similar custom dashboards, you can add the `--nodename <node_name>` argument if you want your dashboard to display metrics for only a single node.
- For troubleshooting, add the `--logfile <log-file>.txt` argument.


## Performance Analyzer configuration

Performance Analyzer uses `/dev/shm` for temporary storage. During heavy workloads on a cluster, Performance Analyzer can use up to 1 GB of space.

Docker, however, has a default `/dev/shm` size of 64 MB. To change this value, you can use the `docker run --shm-size 1gb` flag or [a similar setting in Docker Compose](https://docs.docker.com/compose/compose-file/#shm_size).

If you're not using Docker, check the size of `/dev/shm` using `df -h`. The default value is probably plenty, but if you need to change its size, add the following line to `/etc/fstab`:

```
tmpfs /dev/shm tmpfs defaults,noexec,nosuid,size=1G 0 0
```

Then remount the file system:

```
mount -o remount /dev/shm
```
