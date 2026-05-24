# EIOT Mobile Microgrid Platform

Hardware design and specifications for the EIOT mobile microgrid lab — a portable, open-source energy platform for developing and testing microgrid software and hardware.

## Overview

The mobile microgrid is a self-contained, wheeled power system built around a residential hybrid inverter, a DC battery energy storage system (BESS), and a Gismo Power MEGA solar array unit on wheels. It provides a realistic test environment with grid, generator, solar, storage, and load connections — including an open innovation space for contributors to develop or validate open-source projects.

System architecture is documented in the block diagram: [`mobile-microgrid_block-diagram.drawio`](./mobile-microgrid_block-diagram.drawio) ([PDF export](./mobile-microgrid_block-diagram.drawio.pdf)).

---

## Platform Components

### (01) Hybrid Inverter — Sol-Ark 12K-2P-N

The Sol-Ark 12K-2P-N is a residential hybrid inverter providing grid-tie, off-grid, and backup capabilities.

| Parameter | Value |
|---|---|
| Model | Sol-Ark-12K-P (SKU: 12K-2P) |
| Max continuous AC output | 9,000 W |
| Peak apparent power (10s, off-grid) | 16,000 VA @ 240V |
| Nominal AC voltage | 120/240V, 120/208V, 220V |
| Grid frequency | 50 / 60 Hz |
| Max PV input power (STC) | 13,000 W |
| MPPT trackers | 2 (2 strings each) |
| MPPT voltage range | 150–500V |
| Battery voltage range | 43–63V (nominal 48V) |
| Max battery charge/discharge current | 185 A |
| BMS communication | CANBus & RS485 MODBUS |
| CEC efficiency | 96.5% |
| Backup transfer time | 4 ms |
| Enclosure | IP65 / NEMA 3R |
| Dimensions (H × W × D) | 750 × 450 × 254 mm |
| Weight | 35.4 kg |
| Warranty | 10 years |
| Certifications | UL1741, UL1741SB, IEEE1547, FCC 15B, CA Rule 21, HECO Rule 14H |

The inverter communicates with the Mesh EMS via SunSpec Modbus TCP/IP and with the BESS over closed-loop CAN. Up to 9 units can be stacked in parallel for larger installations.

Full datasheet: [`Hardware Docs/Inverter/SK150-0003-002-12K-2P-N-EN-Datasheet.pdf`](./Hardware%20Docs/Inverter/SK150-0003-002-12K-2P-N-EN-Datasheet.pdf)

---

### (02) DC BESS — Discover HELIOS ESS 16 kWh

The HELIOS ESS is a 16 kWh Lithium Iron Phosphate (LiFePO₄) energy storage system from Discover Energy Systems, featuring a 4th generation Battery Management System (BMS). It is designed for residential, commercial, and off-grid microgrid applications.

#### Electrical Specifications

| Parameter | Value |
|---|---|
| Nominal voltage | 51.2 V |
| Nominal capacity | 314 Ah |
| Usable capacity | 16,080 Wh |
| Depth of discharge | 100% |
| Max continuous charge/discharge current | 200 A |
| Max continuous discharge rate | 10.24 kW (continuous) / 19 kW peak (10s) |
| Peak discharge current (15s) | 300 A RMS |
| Charge absorption voltage | 55.2–56.8 V |
| Charge float voltage | 53.6 V |
| Low voltage disconnect (recommended) | 48 V |
| Lifetime energy throughput | 93 MWh |

#### Mechanical Specifications

| Parameter | Value |
|---|---|
| Dimensions (H × W × D) | 900 × 465 × 247 mm (35.43" × 18.31" × 9.72") |
| Weight | 136 kg (299.83 lb) |
| Enclosure | IP65, galvanized steel |
| Operating temperature | −25°C to 55°C |
| Self-heating (cold climate) | Automatic, active from −25°C to 8°C |

#### Key Features

- 4th generation BMS with overvoltage, over-current, and thermal protection
- Dual fire arrestors and emergency stop for Rapid Shutdown (RSD) integration
- Closed-loop CAN communication with Sol-Ark inverters
- Single-pole 200A breaker on positive terminal
- Quick-connect wall-mount design
- Scalable: up to 3 units in parallel (48 kWh), up to 36 units total (579.6 kWh)

#### Sol-Ark Integration Accessories

A dedicated conduit box (part 950-0067) mounts atop the HELIOS unit and provides Sol-Ark 15K-specific knockouts, quick-connect terminals, integrated LYNK II mounting, and a lockable powder-coated steel enclosure. A generic conduit box (part 950-0077) is also available for other inverters.

Datasheets:
- [`Hardware Docs/BESS/808-0046-helios-datasheet.pdf`](./Hardware%20Docs/BESS/808-0046-helios-datasheet.pdf)
- [`Hardware Docs/BESS/885-0122-helios-ess-parallel-installation.pdf`](./Hardware%20Docs/BESS/885-0122-helios-ess-parallel-installation.pdf)
- [`Hardware Docs/BESS/885-0123-helios-sol-ark-15k-conduit-box-sell-sheet.pdf`](./Hardware%20Docs/BESS/885-0123-helios-sol-ark-15k-conduit-box-sell-sheet.pdf)

> ⚠️ **Safety:** Always follow proper high-voltage handling procedures when connecting battery terminals or working near live DC bus equipment.

---

### (03) PV Array — Gismo Power MEGA, 5.6 kW DC

A freestanding, wheeled Gismo Power MEGA solar array unit providing up to 5.6 kW DC input on a single MPPT channel at 430VDC. The wheeled form factor allows the array to be repositioned independently of the main platform.

---

### (04–05) Load and Grid Subpanels

The system includes separate load and grid subpanels distributing power to the outlets and test loads listed below.

---

### (06) Grid Input

60A / 208V 4-wire grid connection.

---

### (07) EVSE

32A / 208V electric vehicle supply equipment outlet.

---

### (08) Energy Meter — OpenAMI Project

Revenue-grade metering integrated with the [OpenAMI Project](https://github.com/energy-iot/meshems), communicating over RS485/Ethernet/Wi-Fi to the Mesh EMS via MQTT over TCP/IP.

---

### (09–13) Outlet Inventory

| ID | Rating | Type |
|---|---|---|
| 09 | 50A / 208V | NEMA 14-50 |
| 10 | 20A / 120V | 3-wire |
| 11 | 20A / 208V | 3-wire |
| 12 | 20A / 208V | 4-wire |
| 13 | 20A / 208V | 3-wire test load outlet (open innovation space) |

---

### Mesh EMS

The energy management system runs on two Mesh EMS hardware nodes:

- **meshems-sunspec-gateway** — communicates with the Sol-Ark inverter via SunSpec Modbus TCP/IP
- **meshems-openami-metering** — interfaces with the OpenAMI energy meter over RS485/Ethernet/Wi-Fi, publishing data via MQTT

Full EMS documentation: [github.com/energy-iot/meshems](https://github.com/energy-iot/meshems)

---

## Open Innovation Space

The platform includes a dedicated physical space for contributors looking to develop open-source software projects or test new open-source hardware. The test load outlet (13) and open space area (10) are allocated for this purpose.

---

## Repository Structure

```
mobile-microgrid-hw/
├── Hardware Docs/
│   ├── BESS/               # Discover HELIOS ESS datasheets and installation guides
│   └── Inverter/           # Sol-Ark 12K-2P-N datasheet and wiring diagram
├── mobile-microgrid_block-diagram.drawio
└── mobile-microgrid_block-diagram.drawio.pdf
```

---

## Credits

Designed and developed by **Liam O'Brien** and **Glenn Algie**.

---

*EIOT Mobile Microgrid Platform — Rev 001 — 2026-05-23*
