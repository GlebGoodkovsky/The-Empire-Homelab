# The Empire - Homelab

**Builder:** Gleb Goodkovsky

---

## Diagram

***(Temporary diagram to show approxmiate homelab setup)***

![temporarydiagram](assets/tempserverdiagram.jpg)

---

## Current Hardware

<details>
<summary><strong>Raspberry Pi 4 Model B Rev 1.5</strong></summary>

- 4GB RAM
- SanDisk 32GB SD Card
- Raspberry Pi 4 Case, Miuzei Aluminium

![rpi4](assets/rpi4.png)

![sd-card](assets/sd-card.png)

![pi-case](assets/pi-case.png)

</details>

<details>
<summary><strong>Lenovo ThinkCentre M710S SFF</strong></summary>

- i5 6600, 3.20GHz
- 32GB DDR4 RAM (4x8GB - Upgraded)
- 256GB Enterprise SSD (Boot/VMs)

![thinkcentre](assets/thinkcentre.png)

</details>

<details>
<summary><strong>Networking & Power</strong></summary>

- **TP-Link TL-SG105:** 5 Port Gigabit Unmanaged Switch
- **CyberPower ST625U UPS:** 625VA / 360W (USB-HID linked to Host)
- **Telus WiFi Booster:** Primary Uplink
- **DEWENWILS 1ft Extensions:** Power spacing for high-density shelf

![network-switch](assets/network-switch.png)

![UPS-power-bank](assets/ups-power-bank.png)

![wifi-booster](assets/wifi-booster.png)

</details>

---

## Current Software

<details>
<summary><strong>Raspberry Pi 4</strong></summary>

- **OS:** [Raspberry Pi OS](https://www.raspberrypi.com/software/operating-systems/) Lite (64-bit)
- **Services:**
  - [Pi-hole](https://pi-hole.net/)
  - [Tailscale](https://tailscale.com/)

</details>

<details>
<summary><strong>Lenovo ThinkCentre (Node 01)</strong></summary>

- **Hypervisor:** [Proxmox VE](https://www.proxmox.com/en/) 9.1

<details>
<summary><strong>VM 100 — db-mc-server</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 13
- **Specs:** 
  - 8 GB RAM
  - 3 Cores
  - 40 GB Storage
- **Ingress:** [Playit.gg](https://playit.gg/) Tunnel
- **Security:** [Wazuh](https://wazuh.com/) Agent (Endpoint Monitoring)

</details>

<details>
<summary><strong>LXC 101 — uptime-kuma</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 0.5 GB RAM
  - 1 Core
  - 4 GB Storage
- **Role:** Service Heartbeat Monitoring — [Uptime Kuma](https://uptime.kuma.pet/)

</details>

<details>
<summary><strong>LXC 102 — syncthing</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 0.5 GB RAM
  - 1 Core
  - 5 GB Storage
- **Role:** Syncing files — [Syncthing](https://syncthing.net/)

</details>

<details>
<summary><strong>LXC 103 — web-static (Perimeter Defense)</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 0.25 GB RAM
  - 1 Core
  - 4 GB Storage
- **Role:** [Cloudflare](https://www.cloudflare.com/) Tunnel Entrypoint
- **Security Stack:**
  - **IPS:** [CrowdSec](https://www.crowdsec.net/) Security Engine + Firewall Bouncer.
  - **Config:** Nginx Real-IP unmasking active.
  - **Telemetry:** [Wazuh](https://wazuh.com/) Agent active.

</details>

<details>
<summary><strong>LXC 104 — n8n-automation</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12 (Docker)
- **Specs:** 
  - 2 GB RAM
  - 1 Core
  - 12 GB Storage (Resized for stability)
- **Role:** Sovereign [n8n](https://n8n.io/) Logic Engine & [Portainer](https://www.portainer.io/) Management.

</details>

<details>
<summary><strong>LXC 105 — wazuh-server</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 8 GB RAM
  - 2 Cores
  - 30 GB Storage
- **Role:** Security Operations Center (SOC) — [Wazuh](https://wazuh.com/) SIEM/XDR.

</details>

</details>

---

## Operational Protocols

<details>
<summary><strong>The Power Protector</strong></summary>

- **Logic:** NUT (Master) -> n8n Webhook -> Multi-Node SSH Kill Chain -> Host Shutdown.
- **Resilience:** Local-IP triggered to bypass external network failure.

</details>

<details>
<summary><strong>Intelligence Dashboard</strong></summary>

- **Interface:** Telegram Bot
- **Commands:** 
  - `/stats pve`: Stats for Host, VM 100, and LXCs 101-105.
  - `/stats vm`: Specific stats for the Database VM.
  - `/stats pi`: Specific stats for the Raspberry Pi.
  - `/stats help`: Command list.
  - `/update`: Autonomous fleet patching.

</details>

<details>
<summary><strong>Project Sentinel (Defense Fabric)</strong></summary>

- **Perimeter:** CrowdSec actively banning threats on LXC 103 (Web) based on community blocklists and behavior.
- **Intelligence:** [Wazuh](https://wazuh.com/) Agents on VM 100 and LXC 103 feeding vulnerability data and logs to the Central Manager (LXC 105).
- **Hardening:** Memory Shroud applied to Java processes to maintain resource efficiency.

</details>