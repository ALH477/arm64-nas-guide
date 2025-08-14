# ARM64 NAS Build Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Issues](https://img.shields.io/github/issues/username/arm64-nas-guide)](https://github.com/ALH477/arm64-nas-guide/issues)
[![GitHub Stars](https://img.shields.io/github/stars/username/arm64-nas-guide)](https://github.com/ALH477/arm64-nas-guide/stargazers)

This repository provides a comprehensive guide for building low-power, ARM64-based Network Attached Storage (NAS) systems. Optimized for educational applications (e.g., biohacking, aquaponics), these builds emphasize sustainability, open-source compatibility, and cost efficiency. The guide includes tiered parts lists and setup instructions using Debian-based OS like OpenMediaVault (OMV) or TrueNAS Scale.

Ideal for makers, educators, and drop shippers targeting Shopify sales with high margins (100-200%).

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Parts Lists](#parts-lists)
  - [Budget Tier (<$400)](#budget-tier-400)
  - [Mid-Range Tier ($400-800)](#mid-range-tier-400-800)
  - [High-End Tier (>$800)](#high-end-tier-800)
- [Setup Guide](#setup-guide)
- [Drop Shipping Notes](#drop-shipping-notes)
- [Contributing](#contributing)
- [License](#license)

## Overview

ARM64 NAS builds leverage single-board computers (SBCs) for ultra-low power consumption (<10W idle), making them eco-friendly and portable. Components are sourced from reliable vendors like AliExpress, Orange Pi, or Radxa. Wholesale costs are approximately 50-60% of retail, supporting profitable drop shipping.

This guide supports three tiers to suit varying needs, from basic backups to advanced clustered storage.

## Features

- **Low Power Efficiency**: ARM64 architecture ensures minimal energy use, ideal for 24/7 operation.
- **Scalability**: Tiers range from 2-drive RAID to 8-drive clustered setups.
- **Educational Integration**: Includes Docker support for apps like Nextcloud and IoT sensors; bundled tutorials for learning.
- **Open Source**: Compatible with free OS like OMV and TrueNAS Scale; hardware designs available on GitHub.
- **Cost-Effective**: High margins for resellers, with DIY appeal for customization.

## Parts Lists

All prices are approximate (excluding taxes/shipping). Drives are user-supplied. Source from AliExpress or official distributors for bulk discounts.

### Budget Tier (<$400)

For basic home backups or small educational projects. Wholesale: ~$220; Retail: $280-400.

| Component       | Recommendation                | Approx. Cost | Notes                                      |
|-----------------|-------------------------------|--------------|--------------------------------------------|
| SBC            | Orange Pi 5 (8GB RAM)        | $70         | RK3588, PCIe x2, GPIO for sensors.        |
| Storage Adapter| Geekworm X1009 (5-port SATA) | $60         | Supports 5 drives, ~500MB/s.              |
| Case           | Argon THRML (heatsink)       | $40         | Cooling, DIY-friendly.                    |
| PSU            | 27W USB-C (5V/5A)            | $15         | Stable power.                             |
| Storage Boot   | 128GB NVMe SSD               | $25         | Fast OS boot.                             |
| Cooling        | Drive heatsink kit           | $10         | Ensures 24/7 reliability.                 |
| Drives         | 2x 4TB HDD                   | User-supplied| RAID1 in OMV.                             |
| **Total**      |                              | **$220**    | Ideal for starter kits.                   |

### Mid-Range Tier ($400-800)

For multi-user access or automation. Wholesale: ~$360; Retail: $500-800.

| Component       | Recommendation                  | Approx. Cost | Notes                                      |
|-----------------|---------------------------------|--------------|--------------------------------------------|
| SBC            | Orange Pi 5 Plus (16GB RAM)    | $150        | RK3588, 2.5GbE, dual M.2 slots.           |
| Storage Adapter| ASM1166 PCIe-to-6-port SATA    | $50         | >2GB/s aggregate speed.                   |
| Case           | Jonsbo N1 (5-bay)              | $80         | Modular, fits SBC.                        |
| PSU            | 60W PD Charger                 | $30         | Multi-drive support.                      |
| Storage Boot   | 256GB NVMe SSD                 | $40         | Fast boot for labs.                       |
| Cooling        | SBC fan kit                    | $10         | Maintains low temps.                      |
| Drives         | 4x 4TB HDD                     | User-supplied| RAID5 in OMV, Btrfs.                      |
| **Total**      |                                | **$360**    | Scalable, eco-friendly.                   |

### High-End Tier (>$800)

For clustered storage or research labs. Wholesale: ~$1120; Retail: $1000+.

| Component       | Recommendation                  | Approx. Cost | Notes                                      |
|-----------------|---------------------------------|--------------|--------------------------------------------|
| SBC            | 2x Orange Pi 5 Plus (16GB)     | $300        | ARM cluster, <20W idle.                   |
| Storage Adapter| LSI PCIe-to-8-port SATA (x2)   | $300        | Scalable RAID per node.                   |
| Case           | Rackmount 2U                   | $150        | Professional, ventilated.                 |
| PSU            | 120W Industrial                | $50         | Redundant power.                          |
| Storage Boot   | 512GB NVMe SSD (x2)            | $100        | Fast, RAID-capable.                       |
| Networking     | 10GbE NIC (x2) + switch        | $200        | High-speed cluster link.                  |
| Cooling        | Passive radiators (x2)         | $20         | Reliable in rackmount.                    |
| Drives         | 8x 4TB HDD                     | User-supplied| Clustered ZFS RAID.                       |
| **Total**      |                                | **$1120**   | Advanced lab solution.                    |

## Setup Guide

Use OMV for Budget/Mid-Range tiers and TrueNAS Scale for High-End, both ARM64-compatible and free.

1. **Prepare Installation Media**:
   - Download OMV 7 from [openmediavault.org](https://www.openmediavault.org) or TrueNAS Scale from [truenas.com](https://www.truenas.com).
   - Flash to NVMe SSD using Balena Etcher.

2. **Boot and Install**:
   - Configure boot priority via UART/jumpers (refer to Orange Pi documentation).
   - Install OMV (ext4 filesystem, 1GB swap if RAM <16GB) or TrueNAS Scale (ZFS root). Set static IP and hostname (e.g., "edu-nas").

3. **Configure System**:
   - Access web UI: OMV at `http://[NAS-IP]:80`; TrueNAS Scale at `http://[NAS-IP]`.
   - Update system and change default passwords (`admin`/`openmediavault` for OMV; `admin`/`password` for TrueNAS Scale).

4. **Set Up Storage and Shares**:
   - Initialize drives and create filesystems: ext4 (Budget), Btrfs (Mid-Range), ZFS (High-End).
   - Configure RAID: RAID1 (Budget), RAID5 (Mid-Range), clustered ZFS (High-End).
   - Enable SMB/NFS sharing and set user permissions.

5. **Add Features**:
   - Install Docker for educational apps (e.g., Nextcloud, IoT dashboards).
   - Optimize power with `cpufrequtils` (Budget/Mid-Range) or ZFS ARC (High-End).
   - Integrate bundled tutorials from GitHub/Instructables for RAID, networking, and sensors.

6. **Optimize and Test**:
   - Enable SSH with key-based authentication.
   - Monitor via `cockpit` (OMV) or TrueNAS dashboard.
   - Backup configurations and test on QEMU (ARM64 emulation). Verify power draw.

## Drop Shipping Notes

- **Sourcing**: Procure from AliExpress or Orange Pi/Radxa. Use GitHub open-source designs for 3D-printed enclosures to reduce costs.
- **Packaging**: Include QR codes linking to setup videos and PDFs for enhanced user experience.
- **Target Audience**: Tech-savvy makers, IT students, and educators (ages 15-45). Position as sustainable NAS for home labs or classrooms.
- **DIY Customization**: Open-source nature allows modifications (e.g., sensor integrations). 

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/YourFeature`).
3. Commit changes (`git commit -m 'Add YourFeature'`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a Pull Request.

Report issues or suggest improvements via the [Issues](https://github.com/username/arm64-nas-guide/issues) tab.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
