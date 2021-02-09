# Valheim

[Valheim](https://www.valheimgame.com/) is a viking-themed survival game.

## Introduction

This chart creates a single Valheim headless pod, using the [lloesche/valheim-server](https://github.com/lloesche/valheim-server-docker) Docker image.

**Note:** the image will automatically update the server as well as backup the data on a regular basis, see `values.yml` or the details of the image itself for further configuration details.

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

This command deploys a Valheim dedicated server with sane defaults.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

Refer to [values.yaml](values.yaml) for the full run-down on defaults.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release --set var1=Value1,var2=Value2 .
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml .
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [valheim-server](https://github.com/lloesche/valheim-server-docker) image stores persistent data under `/config` attached to the persistent volume.

## Access to the Server

By default, the chart will create the service as a `LoadBalancer` with UDP port mappings (2456, 2457 and 2458 by default).

However in some instances, providers will not allow UDP mapping on load balancers, so the chart can be configured to use `NodePort`. In this case, it is also recommended to use ports within the default node port range. For example: `--set valheimServer.serviceType=NodePort,valheimServer.serverPort=32456`.

Instructions on how to retrieve the IP/port of the server will be displayed after running the `helm` install.

**Note:** `valheimServer.serverPort` (default `2456`) only indicates the first of the three ports defined by the server. The server will automatically open three consecutives ports over UDP, i.e. with the default value of `2456`, this would be: `2456`, `2457` and `2458`.

## Steam Configuration

Go to `view` > `servers` > `add a server` and then `add this address to favorites`.

The port to be used here should be the second one of the three, i.e. with the default value `2456`, the port to connect to in Steam should be `2457`.

**Note:** For reasons that aren't yet competely clear to me, this can be a bit finicky. First, make sure the server is correctly added in Steam. Then start the game > `join game`, select `friends`, then back in Steam, connect to the server, then back in the game again it should automatically connect and you should be able to press `start` and get going.

## Example Setup

```bash
cd valheim-server-helm
kubectl create namespace valheim
helm upgrade --install valheim-server --set valheimServer.serverName="My Valheim Server",valheimServer.worldName="My Valheim World",valheimServer.serverPass="password",valheimServer.serverPublic=0,valheimServer.serviceType=NodePort,valheimServer.serverPort=32456 --namespace valheim .
```

This will setup the Valheim server within the `valheim-server` namespace, make it private and accessible through the node's IP and port (`32456`).