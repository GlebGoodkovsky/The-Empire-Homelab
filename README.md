# The Empire - Homelab

**Builder:** Gleb Goodkovsky

---
**Report Last Updated:** 2026-01-31

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
  - [Uptime Kuma](https://uptime.kuma.pet/) (High Availability Monitor)
  - [Node Exporter](https://prometheus.io/docs/guides/node-exporter/) (Hardware Telemetry)

</details>

<details>
<summary><strong>Lenovo ThinkCentre (Node 01)</strong></summary>

- **Hypervisor:** [Proxmox VE](https://www.proxmox.com/en/) 9.1
- **Kernel Config:** Tuned `vm.max_map_count` (1048576) for OpenSearch stability.
- **Telemetry:** [Prometheus](https://prometheus.io/docs/guides/node-exporter/) (Host metrics)

<details>
<summary><strong>VM 100 — db-mc-server</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 13
- **Specs:** 
  - 8 GB RAM
  - 3 Cores
  - 40 GB Storage
- **Ingress:** [Playit.gg](https://playit.gg/) Tunnel
- **Security:** [Wazuh](https://wazuh.com/) Agent active.
- **Telemetry:** [Prometheus](https://prometheus.io/docs/guides/node-exporter/) active.

</details>

<details>
<summary><strong>LXC 102 — syncthing</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 0.5 GB RAM
  - 1 Cores
  - 5 GB Storage
- **Role:** [Syncthing](https://syncthing.net/) — Syncing files

</details>

<details>
<summary><strong>LXC 103 — web-static</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 256 MB RAM
  - 1 Core
  - 4 GB Storage
- **Role:** [Cloudflare](https://www.cloudflare.com/) Tunnel Entrypoint
- **Security Stack:**
  - **IPS:** [CrowdSec](https://www.crowdsec.net/) Security Engine + Dual-Layer Bouncer (Firewall & [Nginx](https://nginx.org/en/)).
  - **Config:** [Nginx](https://nginx.org/en/) Real-IP unmasking (Cloudflare IPv4/IPv6 trust).
  - **Telemetry:** [Wazuh](https://wazuh.com/) Agent active.

</details>

<details>
<summary><strong>LXC 104 — n8n-automation (The Brain)</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12 (Docker)
- **Specs:** 
  - 4 GB RAM
  - 1 Core
  - 12 GB Storage
- **Role:** Sovereign [n8n](https://n8n.io/) Logic Engine, [Portainer](https://www.portainer.io/), & Log Pipeline Orchestrator.

</details>

<details>
<summary><strong>LXC 105 — wazuh-server (The Eye)</strong></summary>

- **OS:** [Debian](https://www.debian.org/) 12
- **Specs:** 
  - 8 GB RAM
  - 2 Core
  - 30 GB Storage
- **Role:** [Wazuh](https://wazuh.com/) SIEM/XDR — Security Operations Center (SOC)
- **Access:** [Tailscale](https://tailscale.com/) ZTNA (Zero Trust Network Access).
- **Optimization:** JVM Memory Shroud (< 2GB RAM usage).

</details>

</details>

---

## Operational Protocols

<details>
<summary><strong>The Power Protector</strong></summary>

- **Logic:** NUT (Master) -> [n8n](https://n8n.io/) Webhook -> Multi-Node SSH Kill Chain -> Host Shutdown.
- **Resilience:** Local-IP triggered to bypass external network failure.

</details>

<details>
<summary><strong>Intelligence Dashboard</strong></summary>

- **Interface:** [Telegram](https://telegram.org/) Bot
- **Commands:** 
  - `/stats pve`: Stats for Host, VM 100, and LXCs 102-105.
  - `/stats vm`: Specific stats for the Database VM.
  - `/stats pi`: Specific stats for the Raspberry Pi.
  - `/stats help`: Command list.
  - `/update`: Autonomous fleet patching with config-drift protection (`--force-confold`).

</details>

<details>
<summary><strong>Mission Control (Observability)</strong></summary>

- **Telemetry:** Real-time hardware vitals (CPU Load, Stacked RAM, Thermals) scraped by [Prometheus](https://prometheus.io/).
- **Visualization:** Custom [Grafana](https://grafana.com/) dashboard with chained variables (Node -> Instance) for dynamic context switching across the fleet.

</details>

<details>
<summary><strong>Project Sentinel (Defense Fabric)</strong></summary>

- **Perimeter:** [CrowdSec](https://www.crowdsec.net/) actively mitigating real-world botnets at the application layer.
- **SOAR:** [Wazuh](https://wazuh.com/) Level 10+ alerts automatically trigger [n8n](https://n8n.io/) workflows for real-time [Telegram](https://telegram.org/) notification.

</details>