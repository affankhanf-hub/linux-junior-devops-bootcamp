# Linux Junior DevOps Bootcamp

[![Status: Complete](https://img.shields.io/badge/Status-Complete-brightgreen)](.) [![Ubuntu 24.04 LTS](https://img.shields.io/badge/Ubuntu-24.04%20LTS-orange)](.) [![License](https://img.shields.io/badge/License-MIT-blue)](.)

## 📖 Overview

This repository documents my comprehensive journey through a Linux Junior DevOps bootcamp. It serves as a portfolio of hands-on labs, projects, and troubleshooting exercises covering everything from Linux fundamentals to deploying full application stacks.

The goal is to demonstrate practical, job-ready skills in Linux system administration, networking, security, web stacks (LAMP/LEMP), containerization with Docker, and professional documentation with Git.

## 🗂️ Repository Structure

The repository is organized by day and topic, with each directory containing detailed lab reports.

| Directory | Topic | Key Focus Areas |
| :--- | :--- | :--- |
| **[`day01/`](./day01)** | Linux Foundations | Kernel, distributions, filesystem hierarchy, essential CLI commands. |
| **[`day02/`](./day02)** | CLI & Filesystem | Absolute/relative paths, file management, redirection, piping. |
| **[`day03/`](./day03)** | Users, Groups & Permissions | User/group management, `chmod`/`chown` (e.g., 750), the principle of least privilege. |
| **[`day04/`](./day04)** | Processes, Services & Logs | `systemd`, `ps`, `journalctl`, troubleshooting service failures. |
| **[`day05/`](./day05)** | System Maintenance | `apt` package management, system updates, unattended-upgrades. |
| **[`day06/`](./day06)** | Networking Basics | IP addressing, subnetting, CIDR, public vs. private IPs. |
| **[`day07/`](./day07)** | DNS, DHCP & Static IP | DNS resolution, DHCP leases, conceptual static IP configuration. |
| **[`day08/`](./day08)** | Routing, NAT & Firewall | Routing tables, NAT principles, `ufw` firewall implementation. |
| **[`day09/`](./day09)** | TCP Handshake | The 3-way handshake, analyzing `ss`, `nc`, and `curl` outputs. |
| **[`day10/`](./day10)** | Git & Documentation | Professional READMEs, runbook structure, architecture diagrams. |
| **[`day11/`](./day11)** | Docker Basics | Images vs. containers, port mapping, building custom images. |
| **[`day12/`](./day12)** | LAMP Stack Deployment | Deploying Apache, MySQL, PHP (LAMP) on Ubuntu. |
| **[`day13/`](./day13)** | LEMP Stack Deployment | Deploying Nginx, MySQL, PHP-FPM (LEMP). Focus on PHP-FPM integration. |
| **[`day14` & `capstone/`](./capstone)** | Final Capstone Project | End-to-end scenario: Hardening, LEMP stack, Docker, and structured troubleshooting. |
| **[`MiniProject1/`](./MiniProject1)** | Hardening Project | User/group setup, permission hardening, and system baseline configuration. |
| **[`Project2/`](./Project2)** | Connectivity & Firewalls | Deeper dive into firewalls, web server deployment, and service verification. |

## ✅ Key Achievements & Skills Demonstrated

- **Linux Administration:** User/group management, permission hardening (Principle of Least Privilege), `systemd` service control.
- **Networking & Security:** Verified IP routing, DNS resolution, and implemented stateful firewall policies with `ufw`.
- **Web Stack Deployment:** Successfully deployed and troubleshooted both **LAMP** (Apache) and **LEMP** (Nginx) stacks, ensuring PHP processing via Apache `mod_php` and Nginx `PHP-FPM`.
- **Troubleshooting:** Documented real-world failure scenarios for LEMP, Docker, and networking, including root cause analysis and resolution steps.
- **Containerization:** Built and ran custom static HTML sites using `nginx:alpine` Docker containers.
- **Documentation:** Created professional lab reports, runbooks, and architecture diagrams using Markdown.

## 🛠️ Technologies & Tools Used

- **Operating System:** Ubuntu 24.04 LTS (WSL2 & VirtualBox)
- **Web Servers:** Nginx, Apache2
- **Databases:** MySQL, MariaDB
- **Scripting & Processing:** PHP, Bash
- **Containerization:** Docker
- **Security:** UFW (Uncomplicated Firewall)
- **Version Control:** Git & GitHub

## 🚀 How to Use This Repository

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/affankhanf-hub/linux-junior-devops-bootcamp.git
    cd linux-junior-devops-bootcamp
Navigate by topic: Explore the directories listed above to find lab reports and documentation for specific areas of interest.

Review the Capstone: The capstone/day14-capstone-report.md provides the best overview of an end-to-end project, integrating many of the skills learned throughout the bootcamp.

📌 Note on Environment
The majority of these labs were executed in WSL2 (Windows Subsystem for Linux) on Ubuntu 24.04, with some scenarios tested on full VirtualBox VMs to cover concepts like static IP configuration. Any environment-specific limitations are noted directly within the respective lab reports.

🔜 Future Enhancements
Explore Infrastructure as Code (IaC) with Terraform.

Implement CI/CD pipelines for application deployment.

Create architecture diagrams for each major project using Mermaid.

Add a cloud migration plan section for each stack (e.g., mapping LEMP to AWS services).

👤 Author
Affan Khan - GitHub Profile: affankhanf-hub

📄
