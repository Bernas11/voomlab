# VoomLab — Security & Recovery

## Overview

VoomLab is a Debian 13 (Trixie) home server running on an InnJoo Voom Laptop.

Current network configuration:

- Hostname: `serverdecasa`
- LAN IPv4: `192.168.1.107/24`
- Gateway: `192.168.1.254`
- Wi-Fi interface: `wlxc43a3519c90d`
- Permanent Wi-Fi MAC: `c4:3a:35:19:c9:0d`
- IPv6: enabled
- SSH: port `22`
- Uptime Kuma: port `3001`

---

## 1. SSH Security

SSH is hardened through:

`/etc/ssh/sshd_config.d/voomlab-hardening.conf`

Current effective configuration:

```text
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
X11Forwarding no
AllowTcpForwarding no
MaxAuthTries 3
LoginGraceTime 30
AllowUsers bernas

Access model

* Root SSH login is disabled.
* Password authentication is disabled.
* Public-key authentication is required.
* Only the bernas account is allowed to connect.
* SSH access is restricted by the host firewall to the local IPv4 network.

Important

Do not disable or remove the working SSH key before testing another authentication method.

Keep an active emergency SSH session open when making SSH configuration changes.

Validate configuration before reloading:
sudo sshd -t
--
2. Firewall

VoomLab uses nftables.

Configuration:

/etc/nftables.conf

Main firewall table:
inet voomlab

Input policy

The main input chain uses:
policy drop

Allowed traffic includes:

* Loopback
* Established/related connections
* ICMP
* ICMPv6
* Docker network access to Uptime Kuma
* IPv4 LAN access to SSH
* IPv4 LAN access to Uptime Kuma

Current LAN rules
SSH:
192.168.1.0/24 -> TCP 22

Uptime Kuma:
192.168.1.0/24 -> TCP 3001

IPv6

The server currently has a global IPv6 address.

SSH and Uptime Kuma do not have explicit IPv6 allow rules in the VoomLab firewall, so they remain blocked by the input chain’s default-drop policy.

Do not add IPv6 access rules without first deciding whether external IPv6 access is actually required.

Docker

Docker manages additional iptables-nft rules.

Do not manually edit Docker-generated nftables/iptables chains.

Inspect them with:
sudo nft list ruleset
--
3. Docker Security

Docker is installed and running.

Current version:
26.1.5+dfsg1

The Docker security options currently include:
apparmor
seccomp
cgroupns

Uptime Kuma runs as:
uptime-kuma

Docker Compose configuration:
docker/uptime-kuma/compose.yml

Important

Do not delete Docker volumes when troubleshooting containers.

The Uptime Kuma persistent data is stored in the Docker volume:
uptime-kuma_uptime-kuma-data

Container data directory:
/app/data
 
⸻

4. Uptime Kuma

Uptime Kuma is exposed on:
http://192.168.1.107:3001

Container health should be checked with:
docker ps --filter name=uptime-kuma

Expected state:
Up ... (healthy)

The Uptime Kuma database is:
/app/data/kuma.db

Do not manually modify the SQLite database unless absolutely necessary.

The administrator account is:
admin

Never store the administrator password in this repository.
--
## 5. Backup Architecture

VoomLab uses a 3-2-1-inspired backup strategy.

### Local backup

External WD Elements drive:

/mnt/elements

Backup repository:

/mnt/elements/Bernardo/Portofólio/VoomLab/backups

The external drive is NTFS.

Do not run destructive filesystem operations such as:

mkfs
fsck
ntfsfix
ntfsresize

against this disk unless there is a specific, verified recovery requirement.

This disk contains existing personal data in addition to VoomLab backups.

### Cloud backup

Cloud provider:

Google Drive

Remote:

gdrive:VoomLab-Backups

Cloud Restic repository:

rclone:gdrive:VoomLab-Backups

The Restic repository is encrypted.


## 6. Restic Credentials

Restic repository password:

/etc/voomlab/restic-password

Permissions must remain restricted.

Never commit this file or its contents to Git.

The rclone configuration is stored separately:

/home/bernas/.config/rclone/rclone.conf

Never commit this file to Git.


## 7. Automated Cloud Backup

Backup script:

/usr/local/sbin/voomlab-backup-cloud

Systemd service:

voomlab-backup.service

Systemd timer:

voomlab-backup.timer

Schedule:

03:00 daily

The timer uses:

Persistent=true
RandomizedDelaySec=10min

Therefore a missed scheduled run can be triggered after the server comes back online.

### Backup process

The script:

1. Verifies required dependencies.
2. Verifies Docker is running.
3. Verifies Google Drive connectivity.
4. Verifies Uptime Kuma exists and is running.
5. Stops Uptime Kuma.
6. Creates a Restic snapshot.
7. Checks the Restic repository.
8. Starts Uptime Kuma again.
9. Reports success/failure.

A lock prevents overlapping backup executions.

### Manual backup test

sudo systemctl start voomlab-backup.service

Check:

sudo systemctl status voomlab-backup.service --no-pager

Logs:

sudo journalctl -u voomlab-backup.service --no-pager


## 8. Backup Verification

Backups are not considered trustworthy merely because a snapshot exists.

Restic repository checks should be performed:

restic check

Both local and cloud repositories have previously passed integrity checks.

Restore tests have also previously been performed successfully.

When possible, periodically perform a test restore to a temporary directory.


## 9. Lid / Laptop Behaviour

The VoomLab laptop is configured to remain running when the lid is closed.

Configuration:

/etc/systemd/logind.conf.d/voomlab-lid.conf

Current settings:

HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore

Closing the lid therefore does not intentionally suspend or power off the server.

This does not protect against battery depletion or power loss.


## 10. Network Management

Wi-Fi interface:

wlxc43a3519c90d

NetworkManager does not manage this interface.

Configuration:

/etc/NetworkManager/conf.d/voomlab-wifi.conf

The interface uses:

- wpa_supplicant
- dhcpcd

Services:

wpa_supplicant@wlxc43a3519c90d.service
dhcpcd-wlxc43a3519c90d.service

The Wi-Fi interface uses its permanent hardware MAC.

Avoid re-enabling NetworkManager management for this interface without understanding the effect on DHCP identity and IP assignment.


## 11. IP Address

Current server address:

192.168.1.107

The router's DHCP configuration currently uses the option to provide the same address to DHCP clients whenever possible.

However, a guaranteed permanent DHCP reservation for the server has not been independently verified.

Do not change the global DHCP pool merely to force the server to use a particular address.

If the server's IP changes in the future, update services and monitoring configuration accordingly.


## 12. Git Repository

Repository:

~/voomlab

Remote:

git@github.com:Bernas11/voomlab.git

Main branch:

main

The repository contains configuration documentation and Docker configuration.

Never commit:

- passwords
- API keys
- private SSH keys
- rclone configuration
- Restic passwords
- .env files
- personal documents
- private credentials


## 13. Useful Health Checks

### Failed systemd units

systemctl --failed

### Disk space

df -h

### Memory

free -h

### Network

ip -br addr
ip route

### Listening services

sudo ss -lntup

### Firewall

sudo nft list ruleset

### Docker

docker ps

### Uptime Kuma

docker ps --filter name=uptime-kuma

### SSH configuration

sudo sshd -T

### Backup timer

sudo systemctl status voomlab-backup.timer --no-pager

### Backup logs

sudo journalctl -u voomlab-backup.service --no-pager


## 14. Recovery Priorities

If VoomLab experiences a serious failure:

1. Do not immediately reinstall or format anything.
2. Preserve the original storage.
3. Preserve the external backup drive.
4. Determine whether the problem is hardware, filesystem, OS, network, Docker, or application-related.
5. Use the Restic repositories for recovery where appropriate.
6. Test restores before overwriting existing data.
7. Never delete the only known-good copy during recovery.

### Critical principle

The WD Elements drive contains existing personal data.

Treat it as a backup source and valuable data store, not as disposable server storage.


## 15. Current Security Baseline

At the time of this documentation:

- Debian 13 is installed.
- No failed systemd units were reported.
- No pending APT upgrades were reported.
- SSH hardening is active.
- Root SSH login is disabled.
- Password SSH authentication is disabled.
- nftables is active with a default-drop input policy.
- Docker is running with AppArmor and seccomp.
- Uptime Kuma is healthy.
- Automated cloud backups are active.
- Restic repository integrity checks have passed.
- Restore tests have previously succeeded.
- The server is currently using 192.168.1.107.

This document describes the known-good baseline.

Any future security or networking changes should be tested and documented here.
