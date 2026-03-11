# Enterprise Endpoint Adversary Simulation

## ⚠️ Disclaimer / Responsible Use
This repository documents **research, observations, and simulation results** conducted in a **strictly isolated lab environment** as part of a cybersecurity internship.  
All activities were performed for **defensive validation, detection engineering awareness, and security control evaluation**.  
No production systems, real corporate infrastructure, proprietary data, or external third parties were targeted.

> **Redaction Notice:** Organization name, network identifiers, hostnames, usernames, and any environment-specific fingerprints have been anonymized as **`TARGET-CORP`** to prevent attribution and protect operational security.

---

## 📚 Project Structure

This research is divided into multiple phases, each focusing on a different layer of enterprise defense:

| Phase | Focus Area | Description |
|-------|------------|-------------|
| [**Phase 1**](phase1-initial-access/) | Initial Access & Delivery Vectors | Analysis of email gateway rules, attachment filtering, macro execution controls, container-based delivery, HTML smuggling, and link-based delivery. |
| [**Phase 2**](phase2-network-pivesc/) | Network Boundary & Local Privilege Escalation | Post-breach lateral movement, egress filtering validation, reverse tunneling, and local attack surface mapping. |

---

## 🎯 Overall Objective

The purpose of this work is to **map the defensive perimeter** of a modern, hardened enterprise endpoint and measure how well layered controls resist common adversary techniques—from initial delivery to local privilege escalation.

Each phase directory contains its own detailed README with methodology, observations, and defensive insights.

---

## 📁 Repository Structure

```plaintext
.
├── README.md                    # This file
├── phase1-initial-access/        # Initial access vectors (email, web, macro, etc.)
│   └── README.md
├── phase2-network-pivesc/        # Network segmentation, tunneling, local priv esc
│   └── README.md
└── ... (future phases)
