# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ContainerLab-based network verification environment using Open vSwitch bridges to connect container-based network topologies. Written in Japanese documentation. The project simulates L2 switching environments with plans for L3 routing.

## Key Commands

### Build client Docker image
```bash
cd docker-images/client && docker build -t ore-client .
```

### Deploy L2 switch topology
```bash
# Prerequisites: Open vSwitch must be installed and running on the host
# Create OVS bridge first (required before deploy)
sudo ovs-vsctl add-br ovs-br

# Deploy (CLAB_LABDIR_BASE override needed when using multipass mount)
cd containerlab-l2-switch
sudo CLAB_LABDIR_BASE=/home/ubuntu/labs containerlab deploy -t topology.yml
```

### Access nodes and test connectivity
```bash
sudo docker exec -it clab-ubuntu-ovs-switch-ubuntu1 bash
sudo docker exec -it clab-ubuntu-ovs-switch-ubuntu2 bash
# Inside containers: ip addr add 10.0.0.X/24 dev eth1 && ip link set eth1 up
```

## Architecture

- **containerlab-l2-switch/**: L2 topology definition (`topology.yml`) — two Ubuntu nodes (`ubuntu1`, `ubuntu2`) connected via an `ovs-br` Open vSwitch bridge. Uses ContainerLab's `ovs-bridge` kind. The OVS bridge must be pre-created on the host with the same name as the topology node.
- **docker-images/client/**: Custom `ore-client` Docker image (Ubuntu 22.04 + iproute2 + iputils-ping) used as the node image in topologies.
- Planned: `l3-router/` directory for L3 routing environments.
