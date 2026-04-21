# K8s-Galera

*Minimal configuration with managed edge cases to deploy MariaDB Galera cluster in kubernetes cluster*

![Last Commit](https://img.shields.io/github/last-commit/pingmyheart/K8s-Galera)
![Repo Size](https://img.shields.io/github/repo-size/pingmyheart/K8s-Galera)
![Issues](https://img.shields.io/github/issues/pingmyheart/K8s-Galera)
![Pull Requests](https://img.shields.io/github/issues-pr/pingmyheart/K8s-Galera)
![License](https://img.shields.io/github/license/pingmyheart/K8s-Galera)
![Top Language](https://img.shields.io/github/languages/top/pingmyheart/K8s-Galera)
![Language Count](https://img.shields.io/github/languages/count/pingmyheart/K8s-Galera)

## Overview

MariaDB Galera Cluster is a high-availability solution for MariaDB databases. This repository provides a minimal
configuration to deploy a MariaDB Galera cluster in a Kubernetes environment. It includes handling of managed edge cases
to ensure a robust and reliable deployment process.

## Architecture

- **3 MariaDB Nodes**: The cluster consists of three MariaDB nodes that replicate data across each other to ensure
  high availability and fault tolerance.
- **HAProxy**: An optional component that can be used to load balance traffic between the MariaDB nodes and provide a
  single
  endpoint for applications to connect to the database cluster.

## Repository Structure

- `/app/haproxy/deploy` - Contains the deployment YAML file for HAProxy
- `/app/haproxy/service` - Contains the service YAML file for HAProxy
- `/app/mariadb/deploy` - Contains the deployment YAML file for the MariaDB Galera cluster
- `/app/mariadb/service` - Contains the service YAML file for the MariaDB Galera cluster
- `/environment-config` - Contains environment configuration files for the deployment

## Functionality

- **High Availability**: The MariaDB Galera cluster ensures that if one node fails, the remaining nodes can continue to
  operate without interruption.
- **Automatic Failover**: The cluster automatically detects failures and promotes nodes as necessary to maintain
  availability.
- **Monitoring**: The cluster continuously monitors the health of the nodes and provides notifications in case of
  failures.
- **Automatic Configuration**: The deployment includes configurations to handle edge cases, ensuring a smooth and
  reliable deployment process.

## Inner Workings

The following pseudo code describes the inner workings of the entrypoint script for the MariaDB Galera cluster
deployment. It handles various edge cases to ensure a robust and reliable deployment process.

```pseudo
if ! datfile exists; then
    if only one node available; then
        bootstrap new galera cluster
    else
        join existing galera cluster
    fi
else
    if only one node available; then
        if exists safe_to_bootstrap version: then
            configure current node with safe configuration
            bootstrap new galera cluster
        else
            get config with higher seqno or higher recovery position
            configure current node with safe configuration
            bootstrap new galera cluster
        fi
    else
        join existing galera cluster
    fi
fi
```

## Usage

To deploy the MariaDB Galera cluster in your Kubernetes environment, follow these steps:

1. Clone the repository:
   ```bash
   git clone git@github.com:pingmyheart/K8s-Galera.git
   ```
2. Apply Kubernetes configurations:
   ```bash
   kubectl apply -f environment-config/
   kubectl apply -f environment-config/volumes/
   kubectl apply -f environment-config/secrets/
   kubectl apply -f environment-config/config/pdb/
   kubectl apply -f environment-config/config/configmap/
   kubectl apply -f app/haproxy/deploy/
   kubectl apply -f app/haproxy/sercice/
   kubectl apply -f app/mariadb/deploy/
   kubectl apply -f app/mariadb/service/
   ```