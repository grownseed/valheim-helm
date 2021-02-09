# Valheim

[Valheim](https://www.valheimgame.com/) is a viking-themed survival game.

## DEPRECATION NOTICE

This chart is deprecated and no longer supported.

## Introduction

This chart creates a single Valheim headless pod.

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

The [valheim-server](https://github.com/lloesche/valheim-server-docker) image stores the world under /config/worlds.
