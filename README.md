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

- **OS:** Raspberry Pi OS Lite (64-bit)  
  - [Official OS site](https://www.raspberrypi.com/software/operating-systems/)
- **Services:**
  - Pi-hole — [Official site](https://pi-hole.net/)
  - Tailscale — [Official site](https://tailscale.com/)

</details>

<details>
<summary><strong>Lenovo ThinkCentre</strong></summary>

- **Hypervisor:** Proxmox VE 9.1  
  - [Official OS site](https://www.proxmox.com/en/)

</details>

<details>
<summary><strong>VM 100 — db-mc-server</strong></summary>

- **OS:** Debian 13  
  - [Official OS site](https://www.debian.org/)
- **Specs:** 
  - 8 GB RAM
  - 3 Cores
  - 40 GB Storage
- **Ingress:** Playit.gg Tunnel — [Official site](https://playit.gg/)

</details>

<details>
<summary><strong>LXC 101 — uptime-kuma</strong></summary>

- **OS:** Debian 12  
  - [Official OS site](https://www.debian.org/)
- **Specs:** 
  - 0.5 GB RAM
  - 1 Core
  - 4 GB Storage
- **Role:** Service Heartbeat Monitoring — [Uptime Kuma](https://uptime.kuma.pet/)

</details>

<details>
<summary><strong>LXC 102 — syncthing</strong></summary>

- **OS:** Debian 12  
  - [Official OS site](https://www.debian.org/)
- **Specs:** 
  - 0.5 GB RAM
  - 1 Core
  - 5 GB Storage
- **Role:** Syncing files — [Syncthing](https://syncthing.net/)

</details>

<details>
<summary><strong>LXC 103 — web-static</strong></summary>

- **OS:** Debian 12  
  - [Official OS site](https://www.debian.org/)
- **Specs:** 
  - 0.25 GB RAM
  - 1 Core
  - 4 GB Storage
- **Role:** Cloudflare Tunnel Entrypoint — [Cloudflare](https://www.cloudflare.com/)

</details>

<details>
<summary><strong>LXC 104 — n8n-automation</strong></summary>

- **OS:** Debian 12 (Docker)  
  - [Official OS site](https://www.debian.org/)
- **Specs:** 
  - 2 GB RAM
  - 1 Core
  - 8 GB Storage
- **Role:** Sovereign n8n Logic Engine — [n8n](https://n8n.io/)

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
  - `/stats pve`: get stats of pve and all LXCs
  - `/stats vm`: get stats of vm
  - `/stats pi`: get stats of pi
  - `/stats help`: get stat command list
  - `/update`: Manual trigger for safe serverwide update sequence.

</details>