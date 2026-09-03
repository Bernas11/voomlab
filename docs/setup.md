# VoomLab Setup

This document describes the initial setup of the VoomLab server from a fresh Debian 13 installation.

## Hardware

- Intel Celeron N3350
- 4 GB RAM
- 64 GB storage
- Injoo Voom laptop

## Operating System

- Debian 13 (Trixie)
- Minimal installation
- No desktop environment
- SSH server
- Standard system utilities

## Network Configuration

The initial Debian installation did not provide a working network connection.

The Wi-Fi interface was detected after installation, but a network connection had to be configured manually.

### Wi-Fi interface

The wireless interface was identified using:

```bash
ip addr
The interface name starts with wlx.

WPA Supplicant

wpa_supplicant was used to authenticate with the wireless network.

The connection was then configured with DHCP to obtain an IP address.

Network manager decision

NetworkManager was considered as an alternative, but the final setup intentionally remained lightweight using wpa_supplicant and DHCP.

The goal is to minimize unnecessary services and resource usage on the 4 GB machine.

## SSH Access

SSH was configured to allow remote administration without requiring a graphical environment.

### SSH server

The OpenSSH server was enabled and started with:

```bash
sudo systemctl enable --now ssh

The server was then accessed remotely using an SSH client.

User access

Remote administration is performed using the bernas user instead of logging in directly as root.

The bernas user was added to the sudo group:

sudo usermod -aG sudo bernas

A new SSH session is required after changing group membership.

SSH troubleshooting

During the initial setup, SSH failed because the server did not have host keys available.

The missing host keys were generated with:

sudo ssh-keygen -A

After restarting the SSH service, remote access worked correctly.

Root SSH access is intentionally not used for normal administration.
