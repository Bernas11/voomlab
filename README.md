<div align="center">

VoomLab

A small machine. A real lab.

VoomLab is a lightweight homelab built from an old laptop — turning limited hardware into a useful, documented and reproducible home server.

</div>

⸻

The idea

VoomLab is an experiment in doing more with less.

Instead of starting with powerful hardware, the lab starts with a modest machine, a clear purpose and a willingness to understand every layer of the system.

The goal is not maximum performance. It is to build something useful while learning how real systems are deployed, connected, maintained and improved.

┌─────────────────────────────────────────────────────────────┐
│                         V O O M L A B                       │
│                                                             │
│   old laptop  ──►  Linux  ──►  containers  ──►  useful apps │
│                                                             │
│   learn · document · automate · reproduce · improve         │
└─────────────────────────────────────────────────────────────┘

Hardware

<table>
<tr>
<td align="center" width="33%">

CPU

Intel Celeron N3350

</td>
<td align="center" width="33%">

Memory

4 GB RAM

</td>
<td align="center" width="33%">

Storage

64 GB

</td>
</tr>
</table>

The constraints are part of the project. Every service must justify its resource usage.

Stack

Layer	Tools
Operating system	Debian 13
Access	SSH
Networking	wpa_supplicant + DHCP
Runtime	Docker
Orchestration	Docker Compose
Version control	Git

<div align="center">

DEBIAN → SSH → DOCKER → COMPOSE → SERVICES

</div>

What this lab is for

VoomLab is a practical environment for learning and building around:

<table>
<tr>
<td width="50%" valign="top">

Systems

* Linux administration
* Processes and resources
* Storage and backups
* Service management
* Troubleshooting

</td>
<td width="50%" valign="top">

Infrastructure

* Networking fundamentals
* Containers and images
* Reproducible deployments
* Monitoring and maintenance
* Self-hosted services

</td>
</tr>
</table>

Principles

Principle	Meaning
Small first	Start with services the hardware can actually support.
Document everything	If it matters, it belongs in the repository.
Reproducible by default	Prefer configuration that can be rebuilt from scratch.
Understand the layer below	Use abstractions, but learn what they hide.
Improve continuously	Every problem is a chance to make the system clearer.

Current status

FOUNDATION PHASE
✓  old laptop repurposed
✓  Debian 13 installed
✓  Wi-Fi configured
✓  SSH configured
✓  Git configured
✓  Docker installed
✓  Docker Compose ready
□  repository documentation
□  first service deployed
□  persistent storage strategy
□  backup procedures
□  monitoring

Roadmap

* [x]	Repurpose the laptop
* [x]	Install a minimal Debian environment
* [x]	Configure network connectivity
* [x]	Configure SSH access
* [x]	Install Git
* [x]	Install Docker and Docker Compose
* [ ]	Document the initial setup
* [ ]	Deploy the first containerised service
* [ ]	Add persistent volumes
* [ ]	Define backup procedures
* [ ]	Add lightweight monitoring
* [ ]	Document security decisions
* [ ]	Rebuild the lab from documentation alone

Repository structure

voomlab/
├── README.md
├── docs/
│   ├── setup.md
│   ├── networking.md
│   └── backups.md
├── docker/
├── scripts/
└── .gitignore

Goal

Turn limited hardware into a useful, documented and reproducible home server while learning Linux, networking, containers and systems.

VoomLab is not intended to be impressive because of its hardware.

It is intended to be valuable because it is understood, repeatable and always evolving.

⸻

<div align="center">

Build small. Learn deeply. Ship carefully.

VoomLab · self-hosted learning infrastructure

</div>
