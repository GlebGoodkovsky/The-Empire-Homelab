# The Empire - Homelab

**Builder:** Gleb Goodkovsky

---
## Current Hardware

- Raspberry Pi 4 Model B Rev 1.5
    - 4GB RAM
    - SanDisk 32GB SD Card
    - Raspberry Pi 4 Case, Miuzei Aluminium

- Lenovo ThinkCentre M710S SFF Office Desktop PC
    - i5 6600, 3.20GHz
    - 32GB DDR4 RAM (4x8GB - Upgraded)
    - 256GB Enterprise SSD

- Ethernet Network Switch
    - TP-Link TL-SG105
    - 5 Port Gigabit Unmanaged 

- CyberPower ST625U Standby UPS System
    - 625VA/360W
    - 8 Outlets
    - 2 USB Charging Ports
    - USB-HID Data Port (Connected to ThinkCentre)

- Connectivity & Power Spacing
    - Telus WiFi Booster (Existing hardware)
    - DEWENWILS 1 Foot Extension Cords (10-pack)
    - Random Eithernet Cables I found at the moment

---
## Current Software

## Current Software

<details>
<summary><strong>Raspberry Pi 4</strong></summary>

- OS: Raspberry Pi OS Lite (64-bit)
- Services:
  - Pi-hole
  - Tailscale

</details>

<details>
<summary><strong>Lenovo ThinkCentre</strong></summary>

- Hypervisor: Proxmox VE 9.1

<details>
<summary><strong>VM 100 — db-mc-server</strong></summary>

- Operating System: Debian 13
- Specs:
  - 8 GB RAM
  - 3 Cores
  - 40 GB Storage

</details>

<details>
<summary><strong>LXC 101 — uptime-kuma</strong></summary>

- Operating System: Debian 12
- Specs:
  - 0.5 GB RAM
  - 1 Core
  - 4 GB Storage

</details>

<details>
<summary><strong>LXC 102 — syncthing</strong></summary>

- Operating System: Debian 12
- Specs:
  - 0.5 GB RAM
  - 1 Core
  - 5 GB Storage

</details>

<details>
<summary><strong>LXC 103 — web-static</strong></summary>

- Operating System: Debian 12
- Specs:
  - 0.25 GB RAM
  - 1 Core
  - 4 GB Storage

</details>

<details>
<summary><strong>LXC 104 — n8n-automation</strong></summary>

- Operating System: Debian 12
- Specs:
  - 1 GB RAM
  - 1 Core
  - 8 GB Storage

</details>

</details>


---