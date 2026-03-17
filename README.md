# Enterprise Endpoint Adversary Simulation

## ⚠️ Disclaimer / Responsible Use
This repository documents research, observations, and simulation results conducted in a strictly isolated lab environment as part of a cybersecurity internship.  
All activities were performed for defensive validation, detection engineering awareness, and security control evaluation.  
No production systems, real corporate infrastructure, proprietary data, or external third parties were targeted.

> **Redaction Notice:** Organization name, network identifiers, hostnames, usernames, and any environment-specific fingerprints have been anonymized as `TARGET-CORP` to prevent attribution and protect operational security.

---

## 🎯 Overall Objective
The purpose of this work is to map the defensive perimeter of a modern, hardened enterprise endpoint and evaluate how layered controls respond to common adversary techniques, from initial delivery attempts to local privilege escalation scenarios.

Each phase directory contains its own detailed README with methodology, observations, and defensive insights.

---

## 📚 Project Structure
This research is divided into multiple phases, each focusing on a different layer of enterprise defense:

| Phase | Focus Area | Description |
|-------|------------|-------------|
| **Phase 1** | Initial Access & Delivery Vectors | Analysis of email gateway rules, attachment filtering, macro execution controls, container-based delivery, HTML smuggling, and link-based delivery. |
| **Phase 2** | Network Boundary & Local Privilege Escalation | Post-breach boundary analysis, egress filtering validation, reverse tunneling, and local attack surface mapping. |

---

## 📁 Repository Structure

```plaintext
.
├── README.md                 # Main project overview
├── phase1-initial-access/    # Initial access vectors (email, web, macro, etc.)
│   └── README.md
├── phase2-network-pivesc/    # Network segmentation, tunneling, local privilege escalation
│   └── README.md
└── ... (future phases)
