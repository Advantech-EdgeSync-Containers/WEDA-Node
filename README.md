# WEDA-Node
This container includes WEDA Node for edge devices to be integrated with WEDA API, which offers edge-cloud orchestration to enable AIoT network deployment.

# Introduction

## What is WEDA Node?

WEDA Node is an edge software agent that runs on Advantech industrial devices, enabling seamless remote management and monitoring of edge infrastructure. It serves as the intelligent bridge between your edge devices and WEDA Core, providing real-time visibility and control over your distributed IoT deployments.

WEDA Node leverages a digital twin framework to maintain synchronized state between the back-office system and remote edge devices. The framework manages two key states:
- **Desired State**: The target configuration defined by operators in the back-office system
- **Reported State**: The actual current state of the edge device

This bi-directional synchronization ensures that any configuration changes made in the back-office are reliably propagated to edge devices, while the actual device status is continuously reflected back to the cloud for monitoring and validation.

## Business Value

WEDA Node delivers immediate business value by:

- **Reducing Operational Costs**: Eliminate the need for on-site visits by enabling remote configuration, monitoring, and troubleshooting of edge devices
- **Accelerating Time-to-Market**: Deploy and update edge applications remotely through containerized deployment management
- **Improving Reliability**: Proactive monitoring of system health and resource utilization prevents downtime and ensures continuous operations
- **Scaling with Confidence**: Centralized management of thousands of distributed edge devices through a unified digital twin model
- **Enhancing Visibility**: Real-time telemetry and status reporting provide complete transparency into edge infrastructure health and performance

## Key Capabilities

1. **Remote Device Management**: Full lifecycle management of edge devices including configuration, monitoring, and firmware updates
2. **Container Orchestration**: Deploy and manage containerized applications on edge devices without manual intervention
3. **Real-time Monitoring**: Continuous tracking of system resources (CPU, memory, storage) and device health metrics
4. **Bi-directional Synchronization**: Seamless sync between desired cloud configurations and actual device states through digital twin protocol
5. **Sub-Device Integration**: Connect and manage industrial sensors, actuators, and third-party devices through the edge gateway

## Core Components

### Device Management Agent (DMA)

The Device Management Agent is the primary component responsible for maintaining a live connection between your edge device and WEDA Core. It implements the digital twin protocol to ensure cloud and edge are always synchronized.

**What it does:**
- **Configuration Management**: Automatically applies cloud-based configuration changes to edge devices and reports back the actual device state
- **Telemetry Publishing**: Continuously streams device health and operational metrics to the cloud for monitoring and analytics
- **Remote Control**: Executes commands sent from WEDA Core (restart, update, diagnostics) without requiring physical access
- **Application Deployment**: Manages the lifecycle of containerized applications running on the edge device

### Data Agent

The Data Agent is responsible for data collection, gathering vital statistics about the edge device's operational health and performance.

**What it does:**
- Collects system resource metrics including CPU usage, memory consumption, disk space, and network connectivity
- Gathers operational data from running applications and containers
- Provides early warning of resource constraints that could impact operations
- Publishes collected data to the Device Management Agent for cloud visibility
- Helps operations teams optimize resource allocation and predict maintenance needs

## Deployment

### Versions
1. Device Management Agent (DM Agent)

```
harbor.arfa.wise-paas.com/edge-coa/dmagent:v0.1.0-eb.2_20250926.2
```

1. Data Agent 
```
harbor.arfa.wise-paas.com/edge-coa/data_agent:v0.1.0-eb.2.20250806.1
```

### Service Management SOPs

Copy the `Docker Compose File` to device `/home/ubuntu/ops`, and run command
```bash

# Deploy WEDA device credential file to connect to WEDA Core Message Broker.
# Note: Please download the WEDA device credential file from your WEDA Core API. If you experience any issue, please check <LINK> or 
# contact <email>.
# upload the credential file.
$ scp ./authInfo.json <device_ssh_host>:/home/ubuntu/ops/authinfo.json 

# ssh login device and copy the authinfo file to defined file path.
$ ssh <user_name>:<device_ssh_host>

# create docker-compose.yml at /home/ubuntu/ops
$ cd /home/ubuntu/ops

# copy and paste content `Device Management Agent  Docker Compose File` to /home/ubuntu/ops/docker-compose.yml.
$ vi /home/ubuntu/ops/docker-compose.yml
# copy the device credential file to targeted folder.
$ sudo cp /home/ubuntu/ops/authinfo.json /opt/Advantech/dmagent/authinfo.json

# run device agents with docker compose.
$ docker compose up -d

# check docker container service is running.
$ docker compose ps
# Expected to see the dmagent and data agent versions
WARN[0000] /home/ubuntu/ops/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME                 IMAGE                                                             COMMAND                  SERVICE     CREATED              STATUS              PORTS
data_agent_service   harbor.arfa.wise-paas.com/edge-coa/data_agent:xxxx              "dotnet /abp_service…"   dataagent   14 minutes ago       Up 14 minutes
dmagent              harbor.arfa.wise-paas.com/edge-coa/dmagent:xxxx   "/opt/Advantech/dmag…"   dmagent     About a minute ago   Up About a minute

# stop service 
$ docker compose stop

# Stop and remove containers, networks
$ docker compose down -v
```

## WEDA Node agents status checking
```bash
# confirm device service is running
$ docker ps 
CONTAINER ID   IMAGE                                                                                    COMMAND                  CREATED          STATUS              PORTS     NAMES
a871873e848b   harbor.arfa.wise-paas.com/edge-coa/dmagent:v0.1.0-eb.1_20250702.1                        "/opt/Advantech/dmag…"   21 minutes ago   Up About a minute             dmagent
838a9417a9b8   harbor.arfa.wise-paas.com/edge-coa/data_agent:0.1.0.1752144660000                        "dotnet /abp_service…"   21 minutes ago   Up About a minute             data_agent_service
```
