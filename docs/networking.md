VoomLab Networking

This document describes the network configuration of VoomLab, including the host network, Docker networking, exposed services, and firewall configuration.

Network Overview

VoomLab is connected to the local network using Wi-Fi.

Internet
   │
192.168.1.254
   │
   │ Wi-Fi
   │
192.168.1.108
VoomLab
   │
   ├── SSH :22
   │
   ├── Uptime Kuma :3001
   │
   └── Docker
        └── 172.18.0.0/16
             └── Uptime Kuma

Host Network

The wireless interface is:

wlxc43a3519c90d

The VoomLab host currently uses:

IP address: 192.168.1.108/24
Gateway:    192.168.1.254
Network:    192.168.1.0/24

Network information can be inspected with:

ip addr
ip route

The system obtains its network configuration using DHCP.

Docker Networking

Docker creates its own virtual networks.

The default Docker bridge is:

172.17.0.0/16

The Uptime Kuma deployment uses a Docker bridge network:

172.18.0.0/16

The Docker bridge is connected to the host through:

br-1dde512ebab5

Docker automatically manages its own NAT and forwarding rules through iptables-nft.

These rules should not be modified directly.

Exposed Services

The VoomLab host currently exposes two services on the local network:

Port	Service	Purpose
22/tcp	OpenSSH	Remote administration
3001/tcp	Uptime Kuma	Infrastructure monitoring

The containerized Uptime Kuma service is published using Docker:

0.0.0.0:3001 → 172.18.0.2:3001

Firewall

VoomLab uses nftables for host-level packet filtering.

The firewall is configured in:

/etc/nftables.conf

A dedicated table named voomlab is used so that the host firewall remains separate from Docker-managed firewall rules.

The default policy for incoming traffic is:

drop

Allowed traffic includes:

* Loopback traffic
* Established and related connections
* ICMP/ICMPv6
* SSH from the local network
* Uptime Kuma from the local network
* Uptime Kuma access from its Docker network

SSH and Uptime Kuma are restricted to the local network:

192.168.1.0/24

Docker → Host Networking

During firewall testing, Uptime Kuma was initially unable to monitor the host using:

http://192.168.1.108:3001

The request from inside the container timed out.

The cause was the host firewall blocking traffic originating from the Docker network.

The firewall was updated to explicitly allow:

172.18.0.0/16 → TCP/3001

After applying the rule, connectivity was verified from inside the container:

docker exec uptime-kuma curl -I --max-time 5 http://192.168.1.108:3001

The request successfully returned an HTTP response and the Uptime Kuma monitor returned to an operational state.

Design Decisions

Why nftables?

The system already uses iptables-nft through Docker.

Rather than introducing another firewall management layer, VoomLab uses native nftables rules for the host firewall.

Why allow the Docker network?

Uptime Kuma needs to reach the host’s published service in order to monitor it.

The access is restricted to the specific Docker network and TCP port rather than exposing additional services.

Why keep forwarding permissive?

Docker manages its own forwarding and NAT rules.

The initial host firewall therefore leaves the forward chain permissive to avoid interfering with Docker networking.

This can be revisited as the infrastructure grows.

Verification

Useful commands for inspecting the current network configuration:

ip addr
ip route
ss -tulpn

Inspect Docker networks:

docker network ls
docker network inspect <network>

Inspect firewall rules:

sudo nft list table inet voomlab

Check firewall service status:

sudo systemctl status nftables --no-pager

Future Improvements

Potential future networking improvements include:

* Static DHCP lease for VoomLab
* Dedicated management network
* More restrictive Docker forwarding rules
* Internal DNS
* Reverse proxy
* HTTPS
* Remote access through a VPN
* Network monitoring
