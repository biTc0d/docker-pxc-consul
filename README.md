Percona XtraDB Cluster docker image integrated with Consul Service Discovery
===================================

The docker image is available right now at `bitc0d/pxc-consul:5.7`.
The image supports work in Docker Network, including overlay networks,
so that you can install Percona XtraDB Cluster nodes on different boxes.
There is support for the Consul discovery service.

[See it on Docker Hub](https://hub.docker.com/r/bitc0d/pxc-consul)

Basic usage
-----------

The `CLUSTER_NAME` environment variable should be set, and the easiest to do it is:
`export CLUSTER_NAME=cluster1`

The `PXC_SERVICE` environment variable should be set, and the easiest to do it is:
`export PXC_SERVICE=pxc.service.consul`

The script will try to create an overlay network `${CLUSTER_NAME}_net`.
If you want to have a bridge network or network with a specific parameter,
create it in advance.
For example:
`docker network create -d bridge ${CLUSTER_NAME}_net`

The Docker image accepts the following parameters:
* One of `MYSQL_ROOT_PASSWORD`, `MYSQL_ALLOW_EMPTY_PASSWORD` or `MYSQL_RANDOM_ROOT_PASSWORD` must be defined
* The image will create the user `xtrabackup@localhost` for the XtraBackup SST method. If you want to use a password for the `xtrabackup` user, set `XTRABACKUP_PASSWORD`. 
* If you want to use the discovery service, set the address to `DISCOVERY_SERVICE` (and specify the `PXC_SERVICE` address for DNS). The image will automatically find a running cluser by `CLUSTER_NAME` and join to the existing cluster (or start a new one).
* If you want to start without the discovery service, use the `CLUSTER_JOIN` variable. Empty variables will start a new cluster, To join an existing cluster, set `CLUSTER_JOIN` to the list of IP addresses running cluster nodes.
