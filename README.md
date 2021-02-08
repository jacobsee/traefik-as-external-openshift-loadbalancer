# Traefik as a load balancer in front of OpenShift

If you're like me and you just needed a simple TCP load balancer to shove in front of OpenShift to get an install to work (and you already like Traefik!), you can boot a small VM with Docker and `docker-compose` and run this. It expects a `proxy` network to exist, which you can create with `docker network create proxy`.

## Config

Ensure that you have configured `configs/ocp.yml` with IPs that are correct for your network. For the example config, these are the roles for the IPs configured:

| IP | Role |
| --- | --- |
| 10.0.2.1 | master-1 |
| 10.0.2.2 | master-2 |
| 10.0.2.3 | master-3 |
| 10.0.2.4 | worker-1 |
| 10.0.2.5 | worker-2 |
| 10.0.2.6 | worker-3 |
| 10.0.2.10 | bootstrap |

## DNS

_In addition to_ the `bootstrap.<name>.<domain>`, `master<num>.<name>.<domain>`, `worker<num>.<name>.<domain>` DNS entries that the OpenShift installer expects to be pointed at the individual nodes that they represent, the following records should be created and pointed at _this_ instance before you attempt to install the cluster.

| Host | Domain | Type | Value |
| ---- | ------ | ---- | ----- |
| * | `api.<name>.<domain>` | A | <this machine's ip> |
| * | `api-int.<name>.<domain>` | A | <this machine's ip> |
| * | `apps.<name>.<domain>` | A | <this machine's ip> |