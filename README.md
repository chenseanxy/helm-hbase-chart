# HBase chart

Original chart from [warp-poke/hbase-helm](https://github.com/warp-poke/hbase-helm), modified to work with [chenseanxy/helm-hadoop-3](https://github.com/chenseanxy/helm-hadoop-3) chart.

A chart to deploy Hbase with Hadoop using Kubernetes. Heavily inspired by the [Hadoop chart](https://github.com/kubernetes/charts/tree/master/stable/hadoop).

## Getting started

You need:

* Kubernetes
* [Helm](https://helm.sh/)

Required charts: 

 * Zookeeper: `incubator/zookeeper` from helm/charts

 * Hadoop: `chenseanxy/helm-hadoop-3` from [here](https://github.com/chenseanxy/helm-hadoop-3)

## Config

In `values.yaml`:

`hbase.hdfs.name`, `hbase.hdfs.release`: point to your Hadoop deployment

`hbase.zookeeper.quorum`: Zookeeper address:port

## Architecture

This chart is using several functionalities from Kubernetes.

* [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/): at first, it is used as key-value to store elements. Here, we are using it to store config files. Furthermore, we are using it to inject a boostrap.sh to start our container.

  * For every container, we are mouting a container in /tmp based on the content of the ConfigMap (one entry == one file).
  * Entrypoint for every component is the bash called `bootstrap.sh`, which is hold by the ConfigMap.
  * `bootstrap.sh` is copying the files in the ConfigMap to the right location, starting the daemon and tail the logs

* [Headless Service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services): In Kubernetes, every request to a pod a loadbalanced through a proxy by default. But Hbase is directly trying to connect to the RS, so by enabling headless mode, we can directly access the RS container.

* [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/): it is used to deploy stateful applications to Kubernetes.

* [PodDisruptionBudget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/): Allow user to define policy on pod failure.

* [PersistentVolumClaim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/): allows pod to have volumes for data. Used for HDFS.

There's a YAML per role and per functionality. Binding is done through [Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).

## TODO

* Namenode HA
* Hbase Master HA