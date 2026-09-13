# Federated Learning Simulation with Flower and PyTorch

A containerized Federated Learning (FL) simulation built using **Flower, PyTorch, Docker, and MNIST**.

The project implements a central Flower server coordinating two independent client nodes. Each client performs local training on its own partition of the MNIST dataset, after which the server aggregates the locally trained models using **Federated Averaging (FedAvg)**.

## Overview

This project demonstrates the basic end-to-end workflow of Federated Learning:

1. A global model is initialized at the server.
2. Multiple clients connect to the Flower server.
3. The server distributes the current global model parameters.
4. Each client performs local training on its private dataset partition.
5. Clients return their updated model parameters.
6. The server aggregates the client models using FedAvg.
7. The aggregated global model is evaluated on a held-out dataset.
8. The process is repeated for multiple communication rounds.

The entire system runs as separate Docker containers connected through a Docker bridge network.

---
## System Architecture

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#242424"
    primaryTextColor: "#ffffff"
    primaryBorderColor: "#aaaaaa"
    lineColor: "#aaaaaa"
    edgeLabelBackground: "#0d1117"
    secondaryColor: "#242424"
    tertiaryColor: "#242424"
---

flowchart TB

    S["🌐 Flower Server<br/><br/><b>FedAvg Strategy</b><br/>5 Communication Rounds"]

    C1["👤 Client 1<br/><br/>21,000 samples<br/>PyTorch CNN<br/>Local Training"]

    C2["👤 Client 2<br/><br/>15,000 samples<br/>PyTorch CNN<br/>Local Training"]

    E["📊 Server-side Evaluation<br/><br/>24,000 held-out samples"]

    S -->|"Global model parameters"| C1
    S -->|"Global model parameters"| C2

    C1 -->|"Updated parameters"| S
    C2 -->|"Updated parameters"| S

    S -->|"Evaluate aggregated model"| E
```
