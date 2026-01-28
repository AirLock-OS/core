# AirLock OS (alOS)

<div align="center">
  <h1>AirLock OS</h1>
  <h3>We take AI a step further.</h3>
  <p>
    <b>Sovereign. Immutable. Airgapped.</b><br>
    The first Operating System designed exclusively for secure, local AI inference.
  </p>

  [![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
  [![Status: Concept](https://img.shields.io/badge/Status-Architecture_Design-orange)]()
  [![Platform: Alpine](https://img.shields.io/badge/Base-Alpine_Linux-blue)]()
</div>

---

## 🛑 The Problem
The current AI landscape forces a compromise: **Convenience vs. Sovereignty.**
To use state-of-the-art Intelligence (AI), you must send your private data to the cloud. If you want privacy, you must struggle with slow, unoptimized local setups.

## 🔒 The Solution: AirLock
**AirLock OS (alOS)** is a bare-metal hypervisor distribution built for **Absolute State** and **Physical Isolation**. It turns any workstation into a "Sovereign AI Appliance."

We visually represent this philosophy by transforming **AI** (Intelligence) into **AL** (AirLock). We provide the **'L'**—the Lock, the Foundation, the Stability—that raw Intelligence lacks.

### Core Architecture
AirLock uses **Hardware-Assisted Isolation** (IOMMU/VFIO) to physically separate the AI brain from the internet.

| Module | Access | Function |
| :--- | :--- | :--- |
| **The Host (Anchor)** | Read-Only | Immutable Alpine Linux kernel. Wipes state on every boot. |
| **Inference VM** | **Airgapped** | Runs the LLM/GPU. Physically disconnected from the network interface. |
| **Vault VM** | **Airgapped** | Encrypted Vector Database (RAG). Only accepts data via one-way bus. |
| **Scout VM** | Restricted | Browses the web. Can *write* to the Vault but cannot *read* from it. |

---

## ⚡ Tech Stack
* **Hypervisor:** Cloud Hypervisor (Rust)
* **Logic Runtime:** Wasmtime (WebAssembly)
* **Data Bus:** Apache Arrow Flight (Zero-Copy Shared Memory)
* **Base System:** Alpine Linux (Diskless Mode)

---

## 🛠️ Usage (Conceptual)

### 1. The "Data Diode" Protocol
AirLock implements a strict **One-Way Information Flow** for web browsing:

```mermaid
graph LR
    Internet -->|Download| Scout_VM[Scout Module]
    Scout_VM -->|Sanitize & Strip JS| Shared_Mem(Data Diode Buffer)
    Shared_Mem -->|Read Only| RAG_VM[Vault / RAG]
    RAG_VM -.->|X BLOCKED X| Scout_VM
