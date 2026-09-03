<div align="center">

VoomLab

A small machine. A real lab.







































VoomLab is a lightweight homelab built from an old laptop — turning limited hardware into a useful, documented and reproducible home server.

</div>




01 · The idea

VoomLab is an experiment in doing more with less.

Instead of starting with powerful hardware, the lab starts with a modest machine, a clear purpose and a willingness to understand every layer of the system. The goal is not maximum performance. It is to build something useful while learning how real systems are deployed, connected, maintained and improved.

<div align="center">

Plain Text


 old laptop  ─────►  Linux  ─────►  containers  ─────►  useful apps
      │                   │                │                    │
   limited            understand       reproduce            iterate



</div>




02 · Hardware

<table>
<tr>
<td width="33%" align="center">

PROCESSOR

Intel Celeron N3350

</td>
<td width="33%" align="center">

MEMORY

4 GB RAM

</td>
<td width="33%" align="center">

STORAGE

64 GB

</td>
</tr>
</table> <div align="center">

<sub>The constraints are part of the project. Every service must justify its resource usage.</sub>

</div>




03 · Stack

<table>
<tr>
<td width="50%" valign="top">

Core system

Layer
Tool
Operating system
Debian 13
Network
wpa_supplicant + DHCP
Remote access
SSH




</td>
<td width="50%" valign="top">

Services & workflow

Layer
Tool
Runtime
Docker
Orchestration
Docker Compose
Version control
Git




</td>
</tr>
</table> <div align="center">

DEBIAN   →   NETWORK   →   SSH   →   DOCKER   →   SERVICES

</div>




04 · What this lab is for

VoomLab is a practical environment for learning how systems work from the ground up.

<table>
<tr>
<td width="50%" valign="top">

Systems

•
Linux administration

•
Processes and resources

•
Storage and backups

•
Service management

•
Troubleshooting

</td>
<td width="50%" valign="top">

Infrastructure

•
Networking fundamentals

•
Containers and images

•
Reproducible deployments

•
Monitoring and maintenance

•
Self-hosted services

</td>
</tr>
</table>




05 · Principles



Principle
Meaning
01
Small first
Start with services the hardware can actually support.
02
Document everything
If it matters, it belongs in the repository.
03
Reproducible by default
Prefer configuration that can be rebuilt from scratch.
04
Understand the layer below
Use abstractions, but learn what they hide.
05
Improve continuously
Every problem is a chance to make the system clearer.







06 · Current status

<div align="center">

FOUNDATION PHASE

████████████░░░░░░░░ 60%

</div> <table>
<tr>
<td width="50%" valign="top">

Complete




Old laptop repurposed




Debian 13 installed




Wi-Fi configured




SSH configured




Git configured




Docker installed




Docker Compose ready

</td>
<td width="50%" valign="top">

Next up




Repository documentation




First service deployed




Persistent storage strategy




Backup procedures




Monitoring

</td>
</tr>
</table>




07 · Roadmap




Repurpose the laptop




Install a minimal Debian environment




Configure network connectivity




Configure SSH access




Install Git




Install Docker and Docker Compose




Document the initial setup




Deploy the first containerised service




Add persistent volumes




Define backup procedures




Add lightweight monitoring




Document security decisions




Rebuild the lab from documentation alone




08 · Repository structure

Plain Text


voomlab/
├── README.md
├── docs/
│   ├── setup.md
│   ├── networking.md
│   └── backups.md
├── docker/
├── scripts/
└── .gitignore






09 · Goal

<div align="center">


Turn limited hardware into a useful, documented and reproducible home server while learning Linux, networking, containers and systems.

</div>

VoomLab is not intended to be impressive because of its hardware. It is intended to be valuable because it is understood, repeatable and always evolving.




<div align="center">

Build small. Learn deeply. Ship carefully.

<sub>VoomLab · self-hosted learning infrastructure</sub>

</div>


