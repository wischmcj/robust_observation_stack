# Robust Observation Stack Infrastructure

This repository contains the infrastructure code for deploying and managing a modular observation stack alongside the workloads it monitors. The codebase is primarily built using Terraform for infrastructure provisioning and Ansible for configuration management.

![Process Diagram](./archiveteam_process.png)

## High-Level Summary

This codebase provides a complete infrastructure-as-code solution for deploying, managing, and monitoring Docker containers on DigitalOcean servers at scale. The result is a modular observation stack that supports multiple observer and target nodes, each capable of hosting multiple concurrently running Docker containers. It can be configured to run any target image and the accompanying monitoring infrastructure.

The process consists of two main functions:
1. Infrastructure provisioning (Terraform)
   - Creates DigitalOcean droplets for observer and target nodes
   - Manages SSH keys and basic system setup
   - Handles file distribution and initial configuration

2. Configuration Management (Ansible)
   - Sets up monitoring infrastructure (Prometheus, Grafana, Node Exporter)
   - Configures target workloads that are being observed
   - Manages metrics collection and reporting

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

## Terraform Functionality

The Terraform configuration in this repository:

- Provisions DigitalOcean droplets for both observer and target nodes
- Manages SSH key distribution and security
- Handles initial system setup and Docker installation
- Distributes necessary scripts and configuration files
- Sets up networking and security groups
- Manages dependencies between different components
- Supports variable configuration for flexible deployment

Key files:
- `main.tf`: Core infrastructure definitions
- `variables.tf`: Configuration variables
- `outputs.tf`: Output values for the infrastructure
- `providers.tf`: Provider configurations

## Ansible Functionality

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

- **Node Exporter**
  - Installs and configures Node Exporter
  - Exports system metrics
  - Configures security settings

### Target Role
- **Container Workload Setup**
  - Installs Docker and dependencies
  - Configures workloads that the observation stack monitors
  - Sets up project-specific settings

- **Metrics Server**
  - Installs and configures metrics server
  - Exports workload-specific metrics
  - Handles data collection

- **Node Exporter**
  - Installs Node Exporter
  - Configures system metrics collection
  - Sets up security settings

## Getting Started

1. Clone the repository
2. Copy `terraform.tfvars.dist` to `terraform.tfvars` and configure your variables
3. Initialize Terraform: `terraform init`
4. Apply the configuration: `terraform apply`
5. Monitor the deployment through the Grafana dashboard

## Requirements

- Terraform >= 0.12
- Ansible >= 2.9
- DigitalOcean account and API token
- SSH key pair

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request!
