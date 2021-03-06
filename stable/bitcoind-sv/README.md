# bitcoind-sv 

[Bitcoin SV](https://bitcoinsv.io/) full node on kubernetes cluster using helm package manager. 

<img src="./bsv.png" alt="plug" width="200"/>

This repo is:
- forked from: [stable/bitcoind](https://github.com/helm/charts/tree/master/stable/bitcoind) 
- depends on: [docker-bitcoinsv](https://github.com/mrz1836/docker-bitcoinsv) 


### Requirements:

- Kubernetes 1.8+
- PV provisioner support in the underlying infrastructure

### Quickstart:

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release bitcoind-sv
```

The command deploys bitcoind on the Kubernetes cluster in the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured during installation.


### Configuration:

The following table lists the configurable parameters of the bitcoind chart and their default values.

Parameter                 	 	| Description                        				| Default
------------------------------- | ------------------------------------------------- | ----------------------------------------------------------
`image.repository`         		| Image source repository name       				| `gwolf3/docker-bitcoinsv`
`image.tag`                		| `bitcoind` release tag.            				| `0.0.1`
`image.pullPolicy`         		| Image pull policy                  				| `IfNotPresent`
`service.rpcPort`          		| RPC port                           				| `8332`
`service.p2pPort`          		| P2P port                           				| `8333`
`service.testnetPort`      		| Testnet port                       				| `18332`
`service.testnetP2pPort`   		| Testnet p2p ports                  				| `18333`
`service.selector`         		| Node selector                      				| `tx-broadcast-svc`
`persistence.enabled`      		| Create a volume to store data      				| `true`
`persistence.accessMode`   		| ReadWriteOnce or ReadOnly          				| `ReadWriteOnce`
`persistence.size`         		| Size of persistent volume claim    				| `300Gi`
`resources`                		| CPU/Memory resource requests/limits				| `{}`
`configurationFile`        		| Config file ConfigMap entry      				    |
`terminationGracePeriodSeconds` | Wait time before forcefully terminating container | `30`

For more information about Bitcoin configuration please see [the wiki](https://en.bitcoin.it/wiki/Running_Bitcoin#Bitcoin.conf_Configuration_File).

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml stable/bitcoind
```

> **Tip**: You can use the default [values.yaml](values.yaml)

#### Persistence

The bitcoind image stores the Bitcoind node data (Blockchain and wallet) and configurations at the `/bitcoin` path of the container.

By default a PersistentVolumeClaim is created and mounted into that directory. In order to disable this functionality
you can change the values.yaml to disable persistence and use an emptyDir instead.

> *"An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever."*

!!! WARNING !!!

Please NOT use emptyDir for production cluster! Your wallets will be lost on container restart!

#### Customize bitcoind configuration file

```yaml
configurationFile:
  bitcoind.conf: |-
    server=1
    printtoconsole=1
    rpcuser=rpcuser
    rpcpassword=rpcpassword
```
