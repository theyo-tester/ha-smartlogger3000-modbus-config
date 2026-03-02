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

### 📊 Entity Groups & Polling Intervals
To manage hardware resources and network traffic, the entities are grouped by their function with specific scan intervals. 

| Group Name | Entities | Polling Interval |
| :--- | :--- | :--- |
| **Huawei Real-Time Power** | `grid_tied_active_power`, `solar_producion_now`, `battery_inv3_charge_discharge_power` | 5s |
| **Huawei Grid Metrics** | `grid_[a/b/c]_phase_current`, `grid_[a/b/c]_phase_voltage` | 20s |
| | `grid_tied_reactive_power` | 60s |
| **Inverter Active Power** | `inv1_40k_active_power`, `inv2_40k_active_power`, `inv3_10k_batt_active_power` | 20s |
| **ESS & Battery Status** | `real_time_soc`, `ess_battery_status` | 60s |
| | `battery[1/2]_inv3_temperature`, `battery_working_mode`, `battery_inv3_daily_charge/discharge` | 100s |
| **Huawei Energy Yields** | `energy_yield_daily`, `inv1_daily_yield`, `inv2_daily_yield`, `inv3_daily_yield`, `total_energy_yield` | 100s |
| **Huawei Grid Totals** | `total_energy_feed_in_export`, `total_energy_import`, `total_battery_charge/discharge` | 300s |
| **Standalone Entities** | `Plant Status` | 100s |

> [!TIP]
> At the end of the `modbus.yaml` file, I have organized these entities into legacy Home Assistant groups. If you do not use the `group:` component, you can safely remove that section.
> [!NOTE]
> **Missing something?** If you notice any important entities or registers missing from this list that would benefit the community, please feel free to contact me or open an issue!
> 
## 🚀 Installation

To keep your `configuration.yaml` clean and modular, it is recommended to use the **Packages** feature.

1. Download the `modbus.yaml` file from this repository.
2. Put it in your Home Assistant folder, next to the `configuration.yaml`
3. Add (or verify) the following lines in your `configuration.yaml`:

```yaml
homeassistant:
  packages:
    pack_modbus: !include modbus.yaml
```

## 📝 To-Do / Upcoming Improvements

- [ ] **Write Registers:** Add support for controlling inverters directly via Modbus (e.g., limiting power production).
- [ ] **Battery Management:** Include write registers for battery control, such as setting Maximum SOC or Discharge limits.
