# Honeypot Server


## About

[Prometheus](https://prometheus.io) is an open-source monitoring and alerting toolkit designed for reliability and scalability. Initially developed at SoundCloud in 2012, it's now a graduated project under the Cloud Native Computing Foundation (CNCF).

## Git

This project uses a clean and scalable Git workflow to organize the development and deployment lifecycle. By leveraging feature branches and a clear separation between stable and in-progress code, we ensure that our honeypot system remains reliable while encouraging rapid iteration and experimentation.

### Branch Strategy

- **`main`** – The production branch. Only stable, tested code is merged here.
- **`develop`** – Active development branch. All features are integrated here before being released.
- **`feature/*`** – Temporary branches for individual features such as log capture, alerting, or interface improvements.

### Git Flow

{{< mermaid >}}
gitGraph
    commit id: "Initial commit"
    commit id: "Setup Hugo and theme"
    branch develop
    checkout develop
    commit id: "Create honeypot layout and structure"
    branch feature/logging
    checkout feature/logging
    commit id: "Implement traffic logging"
    checkout develop
    merge feature/logging
    branch feature/alerting
    checkout feature/alerting
    commit id: "Add alert system for intrusions"
    commit id: "Connect webhook for alert delivery"
    checkout develop
    merge feature/alerting
    branch feature/visualization
    checkout feature/visualization
    commit id: "Add data visualization dashboard"
    checkout develop
    merge feature/visualization
    commit id: "Final testing and cleanup"
    checkout main
    merge develop
    commit id: "Release v1.0"
{{< /mermaid >}}

## Architecture

```mermaid
graph TD

subgraph "Jobs"
  J1[Jobs/exporters]
  J2[Short-lived jobs]
end

subgraph "Service Discovery"
  SD1[Kubernetes]
  SD2[file_sd]
end

subgraph "Prometheus Server"
  RET[Retrieval]
  TSDB[TSDB]
  HTTP[HTTP server]
end

subgraph "Storage"
  HDD[HDD/SSD]
end

subgraph "Visualization"
  UI[Prometheus web UI]
  G[Grafana]
  API[API clients]
end

subgraph "Alerting"
  AM[Alertmanager]
  PD[pagerduty]
  Email[Email]
  ETC[etc]
end

subgraph "Pushgateway"
  PG[Pushgateway]
end

%% Data Flow
J1 -->|pull metrics| RET
J2 -->|push metrics at exit| PG
PG -->|pull metrics| RET
SD1 -->|discover targets| RET
SD2 -->|discover targets| RET
RET --> TSDB
TSDB --> HTTP
TSDB --> HDD
HTTP --> UI
HTTP --> G
HTTP --> API
HTTP -->|push alerts| AM
AM -->|notify| PD
AM -->|notify| Email
AM -->|notify| ETC


```


---

> Author: [ProxyGeek](https://github.com/Pr0xyG33k)  
> URL: https://Pr0xyG33k.github.io/projects/honeypot/  

