# Multi-Channel Temperature Controller & Modbus TCP Hardware Integration

Engineering documentation, hardware wiring schematics, and field commissioning logs for integrating multi-channel Autonics TM4 temperature controllers into an industrial PLC/SCADA network via Modbus TCP.

---

## Key Technical Highlights

* **Protocol Migration for Latency Reduction:** Migrated field instrument communication from legacy RS-485 Modbus RTU to Ethernet-based Modbus TCP, eliminating serial polling bottlenecks.
* **Network Acquisition Cycle Optimization:** Reconfigured HMI tag update frequencies from high-rate polling to `Upon Change` with a `250ms` fallback cycle, solving distributed network delay issues.
* **Hardware Troubleshooting & Schematic Redesign:** Verified physical sensor terminal connections, diagnosed wiring discrepancies, and redrawn electrical wiring diagrams for multi-zone heating control circuits.

---

## Hardware Interfacing & Architecture

```mermaid
graph TD
    subgraph Multi-Zone Field Heating
        TC1[Thermocouple Sensor Zone 1] --> TM4[Autonics TM4 Controller]
        TC2[Thermocouple Sensor Zone 2] --> TM4
        TM4 --> SSR[Solid State Relays]
    end

    subgraph Communication Bridge
        TM4 -- Modbus TCP / Ethernet --> Gateway[Industrial Switch]
        Gateway -- Ethernet --> PLC[Siemens S7 PLC / WinCC]
    end
