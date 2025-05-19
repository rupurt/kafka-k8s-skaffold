# kafka-k8s-skaffold

[Apache Kafka](https://kafka.apache.org/) deployment on Kubernetes with Skaffold

## Description

This repository provides a development environment managed as a [Nix flake](https://wiki.nixos.org/wiki/Flakes). It contains:

- [argc](https://github.com/sigoden/argc) - A Bash CLI framework, also a Bash command runner
- [k3d](https://k3d.io/stable) - Lightweight wrapper to run k3s in docker
- [Skaffold](https://skaffold.dev) - Handles the workflow for building, pushing and deploying your application
- [Strimzi Kafka Operator](https://strimzi.io) - Kafka kubernetes operator as a helm chart
- [Kafka proxy](https://github.com/grepplabs/kafka-proxy) - Proxy connections to a Kafka cluster
- [Redpanda Console](https://github.com/redpanda-data/console) - Developer-friendly UI for managing your Kafka/Redpanda workloads

## Requirements

- [Nix](https://github.com/DeterminateSystems/nix-installer)
- [Docker](https://www.docker.com/)

## Development Commands

```console
> dev
USAGE: dev <COMMAND>

COMMANDS:
  setup     Ensures configuration has been initialized
  k3d       Manage local k3d cluster
  skaffold  Continuous development for kubernetes
```

## Usage

### 1. Create a Kubernetes Cluster Using K3D

```console
> dev k3d create
Running:
  k3d cluster create kafka-k8s-skaffold     --image rancher/k3s:v1.31.4-k3s1     --k3s-arg '--disable=traefik@server:*'     --port '80:80@loadbalancer'

INFO[0000] portmapping '80:80' targets the loadbalancer: defaulting to [servers:*:proxy agents:*:proxy]
...
```

### 2. Deploy the Kafka Cluster with Skaffold

```console
> dev skaffold kafka run
Running:
  skaffold run     --port-forward=user     -f skaffold/kafka.yaml

No tags generated
Starting test...
...
```

## Cleanup

### 1. Delete the Kafka Cluster with Skaffold

```console
> dev skaffold kafka delete
Running:
  skaffold delete     -f skaffold/kafka.yaml

Cleaning up...
release "strimzi-kafka-operator" uninstalled
release "kafka-cluster" uninstalled
release "kafka-proxy" uninstalled
release "redpanda-console" uninstalled
```

### 2. Optionally Delete the K3D Cluster

```console
> dev k3d delete
Running: k3d cluster delete kafka-k8s-skaffold
INFO[0000] Deleting cluster 'kafka-k8s-skaffold'
INFO[0001] Deleting cluster network 'k3d-kafka-k8s-skaffold'
INFO[0001] Deleting 1 attached volumes...
INFO[0001] Removing cluster details from default kubeconfig...
INFO[0001] Removing standalone kubeconfig file (if there is one)...
INFO[0001] Successfully deleted cluster kafka-k8s-skaffold!
```
