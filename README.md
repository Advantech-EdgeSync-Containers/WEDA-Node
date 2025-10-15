# WEDA-Node
This container includes WEDA Node for edge devices to be integrated with WEDA API, which offers edge-cloud orchestration to enable AIoT network deployment.




# Introduction

## What is WEDA Node?

WEDA Node is an edge software agent that runs on Advantech industrial devices, enabling seamless remote management and monitoring of edge infrastructure. It serves as the intelligent bridge between your edge devices and WEDA Core, providing real-time visibility and control over your distributed IoT deployments.

WEDA Node leverages a digital twin framework to maintain synchronized state between the back-office system and remote edge devices. The framework manages two key states:

- **Desired State**: The target configuration defined by users in the back-office cloud
- **Reported State**: The actual current state of the edge device

This bi-directional synchronization ensures that any configuration changes made in the back-office are reliably propagated to edge devices, while the actual device status is continuously reflected back to the cloud for monitoring and validation.


<img width="4444" height="3380" alt="WEDA_arch_plot" src="https://github.com/user-attachments/assets/b6f08a06-e4d3-4785-ab7c-fc8261541cfe" />



## Business Value

WEDA Node delivers immediate business value by:

| Benefit | Description |
|----------|-------------|
| **Reducing Operational Costs** | Eliminate the need for on-site visits by enabling remote configuration, monitoring, and troubleshooting of edge devices. |
| **Accelerating Time-to-Market** | Deploy and update edge applications remotely through containerized deployment management. |
| **Improving Reliability** | Proactive monitoring of system health and resource utilization prevents downtime and ensures continuous operations. |
| **Scaling with Confidence** | Centralized management of thousands of distributed edge devices through a unified digital twin model. |
| **Enhancing Visibility** | Real-time telemetry and status reporting provide complete transparency into edge infrastructure health and performance. |


## Key Capabilities

| Feature | Description |
|----------|-------------|
| **Remote Device Management** | Full lifecycle management of edge devices including configuration, monitoring, and firmware updates. |
| **Container Orchestration** | Deploy and manage containerized applications on edge devices without manual intervention. |
| **Real-time Monitoring** | Continuous tracking of system resources (CPU, memory, storage) and device health metrics. |
| **Bi-directional Synchronization** | Seamless sync between desired cloud configurations and actual device states through digital twin protocol. |


## Core Components

### Device Management Agent (DMA)

The Device Management Agent is the primary component responsible for maintaining a live connection between your edge device and WEDA Core. It implements the digital twin protocol to ensure cloud and edge are always synchronized.

**What it does:**
| Function | Description |
|-----------|-------------|
| **Configuration Management** | Automatically applies cloud-based configuration changes to edge devices and reports back the actual device state. |
| **Telemetry Publishing** | Continuously streams device health and operational metrics to the cloud for monitoring and analytics. |
| **Remote Control** | Executes commands sent from WEDA Core (restart, update, diagnostics) without requiring physical access. |
| **Application Deployment** | Manages the lifecycle of containerized applications running on the edge device. |


### Data Agent

The Data Agent is responsible for data collection, gathering vital statistics about the edge device's operational health and performance.

**What it does:**
| Capability | Description |
|-------------|-------------|
| **System Resource Monitoring** | Collects system resource metrics including CPU usage, memory consumption, disk space, and network connectivity. |
| **Application Data Collection** | Gathers operational data from running applications and containers. |
| **Proactive Alerts** | Provides early warning of resource constraints that could impact operations. |
| **Cloud Data Publishing** | Publishes collected data to the Device Management Agent for cloud visibility. |
| **Operational Optimization** | Helps operations teams optimize resource allocation and predict maintenance needs. |


# Deployment

## Versions
 * Device Management Agent (DM Agent)

```bash
harbor.arfa.wise-paas.com/edge-coa/dmagent:v0.1.0-eb.2_20250926.2
```

* Data Agent 
```bash
harbor.arfa.wise-paas.com/edge-coa/data_agent:v0.1.0-eb.2.20250806.1
```

## WEDA Node Deployment SOP

1. Registers your device to WEDA Core and downloads WEDA Node credential file from the WEDA Core. 
    **Please download the WEDA node credential file (authInfo.json) from your WEDA Core API.** If you experience any issue here, please contact Candice Wu at Candice.Wu@advantech.com.tw.

2. Uploade the WEDA Node credential file to the edge device.
```bash
 $ scp ./authInfo.json <device_ssh_host>:/home/ubuntu/ops/authinfo.json 
 ```
3. SSH login the edge device
```bash
 $ ssh <device_ssh_host>
 $ sudo cp /home/ubuntu/ops/authinfo.json /opt/Advantech/dmagent/authinfo.json
```
4. Deploy and run `docker-compose` file for WEDA Node

   ***!!!Please make sure you already upload the WEDA node credential file to device before following steps!!!***

```bash
# copy and paste content `WEDA Node Docker Compose File` to /home/ubuntu/ops/docker-compose.yml.
$ vi /home/ubuntu/ops/docker-compose.yml

# run device agents with docker compose.
$ docker compose up -d
```

5. Check WEDA Node running status
```bash
$ docker compose ps
# Expected to see the dmagent and data agent versions
WARN[0000] /home/ubuntu/ops/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME                 IMAGE                                                             COMMAND                  SERVICE     CREATED              STATUS              PORTS
data_agent_service   harbor.arfa.wise-paas.com/edge-coa/data_agent:xxxx              "dotnet /abp_service…"   dataagent   14 minutes ago       Up 14 minutes
dmagent              harbor.arfa.wise-paas.com/edge-coa/dmagent:xxxx   "/opt/Advantech/dmag…"   dmagent     About a minute ago   Up About a minute
```

6. Stop WEDA Node
```bash
# stop service 
$ docker compose stop

# Stop and remove containers, networks
$ docker compose down -v
```

Copyright © 2025 Advantech Corporation. All rights reserved.
