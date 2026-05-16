<div align="center">
  
  # ☁️ VMware vSphere Infrastructure Implementation Project ☁️

  <p align="center">
    <a href="https://www.vmware.com/products/vsphere.html"><img src="https://img.shields.io/badge/VMware-vSphere_6.7-0091DA?style=for-the-badge&logo=vmware&logoColor=white" alt="vSphere"></a>
    <a href="https://ubuntu.com/"><img src="https://img.shields.io/badge/Ubuntu-24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu"></a>
    <a href="#"><img src="https://img.shields.io/badge/Virtualization-Nested-brightgreen?style=for-the-badge&logo=virtualbox&logoColor=white" alt="Virtualization"></a>
    <a href="#"><img src="https://img.shields.io/badge/Storage-NFS-ff69b4?style=for-the-badge&logo=linux&logoColor=white" alt="NFS"></a>
  </p>
</div>

---

## 📌 Overview
This repository contains the documentation and architectural details for a comprehensive VMware vSphere 6.7 infrastructure implementation. The project involves designing and deploying a highly available private cloud environment from scratch using nested virtualization.

This deployment demonstrates core virtualization concepts, enterprise-level datacenter design, and the configuration of advanced vSphere features to ensure high availability, load balancing, and fault tolerance.

## 🏗️ Architecture & Infrastructure
The environment is built using nested virtualization on VMware Workstation across two physical machines, simulating a complete enterprise datacenter.

### ⚙️ Core Components
* **Hypervisors:** 3x ESXi 6.7 Hosts (`ESXi-0`, `ESXi-1`, `ESXi-2`)
* **Management:** vCenter Server Appliance (VCSA) deployed on `ESXi-0`
* **Storage:** Centralized Linux NFS Shared Storage Server (Ubuntu)
* **Guest OS:** Ubuntu Linux 24.04

### 🔀 Networking Design (Traffic Separation)
A deliberate two-switch networking architecture was implemented to separate traffic types, mirroring enterprise best practices:
* **vSwitch0 (Management & VM Traffic):** Handles ESXi management traffic and guest virtual machine network communication.
* **vSwitch1 (Storage, vMotion & FT Traffic):** Attached to a dedicated physical/virtual NIC to isolate storage I/O, live migration, and fault tolerance synchronization traffic on a separate subnet.

### 🌐 IP Addressing Scheme
| Device | IP Address | Role |
|:---|:---:|:---|
| **ESXi-0** | `10.29.24.90` | ESXi Host 00 |
| **ESXi-1** | `10.29.24.91` | ESXi Host 01 |
| **ESXi-2** | `10.29.24.92` | ESXi Host 02 |
| **vCenter Server** | `10.29.24.95` | Centralized Management |
| **NFS-Storage** | `10.29.24.130` | Linux NFS Shared Storage Server |

> **Network:** `10.29.24.0` | **Subnet Mask:** `255.255.255.0` | **Gateway:** `10.29.24.80`

## 🚀 Key Features Implemented

* 🛡️ **vCenter Clustering:** Grouped ESXi hosts into a single logical cluster (`ITI-SA-Cluster`), pooling CPU, memory, and storage resources for centralized management.
* 💾 **Shared Storage & Content Library:** Configured an NFS datastore accessible by all hosts to enable advanced cluster features. Created a centralized local Content Library (`ISO-REPO-NFS`) for ISO images and templates.
* ♻️ **VM Lifecycle Operations:** Successfully deployed VMs, took point-in-time snapshots, cloned VMs, and converted VMs into reusable deployment templates with Customization Specifications.
* ⚡ **vMotion (Live Migration):** Configured a dedicated VMkernel adapter for vMotion, allowing zero-downtime migration of running VMs between ESXi hosts.
* 🏥 **High Availability (HA):** Configured cluster-level HA to protect against hardware failures. Successfully validated by simulating a host failure, resulting in automated VM restarts on surviving hosts.
* ⚖️ **Distributed Resource Scheduler (DRS):** Enabled Fully Automated DRS to continuously monitor and balance CPU and memory workloads across the cluster using vMotion.
* 🔄 **Fault Tolerance (FT):** Configured continuous availability for critical VMs by maintaining a live, lockstep shadow instance on a secondary host, ensuring zero downtime and no data loss during a primary host failure.

## 🛠️ Challenges & Troubleshooting
* **Nested Virtualization Requirements:** Required enabling hardware virtualization flags (`VT-x/EPT`) on ESXi VMs to allow running guest VMs inside the virtualized hypervisors.
* **Network Isolation Constraints:** Overcame physical NIC limitations by adding secondary virtual network adapters mapped to specific VMnet subnets, enabling the creation of `vSwitch1` without extra physical hardware.
* **Cross-Hardware vMotion:** Aligned VMnet subnets carefully across the physical machines to ensure vMotion VMkernel adapters could communicate seamlessly over the physical LAN.

---

## 👥 Project Team
This project was completed as part of the **ITI_SYSTEM_ADMIN_46 ALEXANDRIA** intake.

**Team Members:**
* Omar Hesham
* Ahmed Kamel
* Mohamed Morsi
* Marwan Tarek

**Supervised by:** ENG. Ekram Abdelwahab Nour

<div align="center">
  <sub>Built with ❤️ and virtualization.</sub>
</div>
