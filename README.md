# Segmented Hardening Lab Environment (IaC)

This repository contains a **Vagrant** configuration to automatically spin up a local, isolated, and segmented network environment inside VirtualBox. It is designed to simulate a real-world enterprise network topology for practicing system hardening and network firewall configurations.

## Network Architecture

The environment consists of three virtual machines (Ubuntu Server 20.04) split into distinct zones:

1. **Firewall/Router (`10.0.1.1` & `10.0.2.1`):** Acts as the default gateway between the networks. It has IP forwarding enabled to route traffic and manage firewall rules.
2. **Admin Client (`10.0.1.10`):** Simulates the secure administrator workstation located in the management zone (`Admin_Net`).
3. **Hardened Server (`10.0.2.10`):** The target machine to be hardened, completely isolated inside the server zone (`Server_Net`).

## Prerequisites

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

## Getting Started

1. Clone this repository:

```bash
   git clone <your-repo-url>
   cd <repo-name>
```
