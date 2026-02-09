# Robust Observation Stack Infrastructure

This repository contains the infrastructure code for deploying and managing a modular observation stack alongside the workloads it monitors. The codebase is primarily built using Terraform for infrastructure provisioning and Ansible for configuration management.

![Process Diagram](./archiveteam_process.png)

## High-Level Summary

This codebase provides a complete infrastructure-as-code solution for deploying, managing, and monitoring Docker containers on DigitalOcean servers at scale. The result is a modular observation stack that supports multiple observer and target nodes, each capable of hosting multiple concurrently running Docker containers. It can be configured to run any target image and the accompanying monitoring infrastructure. 

   - Sets up monitoring infrastructure (Prometheus, Grafana, Node.js metrics server)
   - Configures export infra (cAdvisor, Node Exporter) on worker nodes 
   - Builds Docker containers from specified images on worker nodes
   - Starts Containers

## Purpose

This stack enables the rapid deployment of an observation infrastructure that can collect, visualize, and alert on metrics from one or more target workloads.

## Architecture Overview

The infrastructure consists of two types of nodes:

1. Observer Nodes
   - Run Prometheus for metrics collection
   - Host Grafana for visualization
   - Include Node Exporter for system metrics
   - Monitor the target workloads

2. Target Nodes
   - Run the containerized workloads that the observation stack monitors
   - Include Node Exporter for metrics
   - Run an optional metrics server for workload-specific statistics
   - Can be scaled horizontally based on needs

## Functionality Details

The Ansible playbooks handle the configuration and setup of both observer and target nodes.

### Observer Role
- **Prometheus Setup**
  - Installs and configures Prometheus
  - Sets up scraping targets for all nodes
  - Configures retention and storage

- **Grafana Setup**
  - Installs Grafana
  - Configures dashboards
  - Sets up data sources

- **Metrics Server**
  - Installs and configures metrics server
  - Assists in aggregation of workload-specific metricss

- **Node Exporter**
  - Installs and configures Node Exporter
  - Exports system metrics
  - Configures security settings

### Target Role
- **Container Workload Setup**
  - Installs Docker and dependencies
  - Configures workloads that the observation stack monitors
  - Sets up project-specific settings

- **Node Exporter**
  - Installs Node Exporter
  - Configures system metrics collection
  - Sets up security settings
