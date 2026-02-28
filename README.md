# Home Assistant Modbus Configuration for Huawei SmartLogger 3000

This repository contains a curated selection of Modbus registers compatible with the **Huawei SmartLogger 3000**, optimized for Home Assistant.

## 🛠️ Prerequisites

Before implementing this configuration, ensure your SmartLogger is prepared:

1. **Enable Modbus TCP:** Turn this option **ON** in the SmartLogger settings.
2. **Static IP:** Assign a fixed IP address to your SmartLogger to prevent connection loss.
3. **Address Mode:** In the Modbus TCP configuration, ensure you are using **Logical Addressing**.

> [!IMPORTANT]
> **Note on the SmartLogger ID:** > According to the official Reference Documentation: *"In the Modbus-TCP communications protocol, the logical device ID for the SmartLogger itself is fixed to **0**."*

## 📐 System Architecture

This configuration is based on my specific hardware setup. You may need to adjust the slave IDs to match your own:
* **Three (3) Inverters** at specific logical slave addresses.
* **One (1) Smart Meter**.
* **One (1) Battery** (connected to the 10kW inverter).

**Action Required:** You must identify the logical addresses for your specific devices (inverters, meters, etc.) and update the `slave:` values in the `modbus.yaml` file accordingly. 

As you will notice, the registers for the inverters and battery data are identical to the official Huawei Modbus specifications used for direct inverter connections. The only functional difference when communicating via the SmartLogger is the **Slave ID** used to route the request to the correct downstream device.


## ✨ Features & Useful Notes

* **Load Power Calculation:** The SmartLogger does not provide "Actual Load Power" as a direct register. I have included a **Template Sensor** in this config to calculate this value automatically.
* **Entity Grouping:** At the end of the file, I have organized the entities into groups. If you do not use the legacy `group:` component or prefer a different organization, you can safely remove that section.
* **Optimized Selection:** This config focuses on the most useful registers for Home Assistant dashboards and Energy Management, rather than pulling every single available data point.

## 🚀 Installation

To keep your `configuration.yaml` clean and modular, it is recommended to use the **Packages** feature.

1. Download the `modbus.yaml` file from this repository.
2. Put it in your Home Assistant folder, next to the `configuration.yaml`
3. Add (or verify) the following lines in your `configuration.yaml`:

```yaml
homeassistant:
  packages:
    pack_modbus: !include modbus.yaml
