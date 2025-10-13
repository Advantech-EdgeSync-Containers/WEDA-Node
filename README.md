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
