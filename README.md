### Temperature Control Interface and Modbus RTU Polling Optimization for TM4 Controllers

### Overview

Temperature sensors (RTD) on this line were hard to maintain because they were not integrated with the PLC or the HMI: no central view, and no logged history to diagnose problems. This project connects TM4 temperature controllers over Modbus RTU, gives operators a clear control interface, and adds the data logging needed to move up to a higher SCADA level.

### System architecture
```RTD sensors → TM4 controllers → Modbus RTU (RS-485) → Siemens S7-200 SMART (Modbus master) → OPC → WinCC Explorer (operator HMI) and WinCC Runtime Advanced in TIA Portal V16 (reports and formatting)```



The S7-200 SMART polls the TM4 controllers over Modbus RTU. Since Modbus RTU handles one request at a time, the order of requests in the PLC program sets how fresh and how consistent each value is. Polling order is configured in the ladder program in STEP 7-Micro/WIN SMART.

### Key features

- Intuitive parameter control screen: clear labels, grouped parameters, and display values visually separated from input fields.
- Auto/Manual control mode selection.
- PID control using the PID function built into the TM4.
- Advanced data logging using VBScript in Runtime. [In progress]
- Automatic Conditional Formatting to highlight abnormal temperature states. [In progress]

### Challenges and solutions

- **Challenge: Sensors were not integrated, so maintenance was slow.**
  **Solution:** Brought TM4 data into the PLC/HMI over Modbus RTU and gave operators one screen for monitoring and parameter control.

- **Challenge: Parameters were hard for operators to understand.**
  **Solution:** Redesigned the control screen with clear grouping and labels, and separate styles for read-only values and editable inputs.

- **Challenge: MV (manipulated value) Read and Write were out of sync after switching between Auto and Manual.**
  After deployment, the MV shown on the HMI did not match the MV written to the controller right after a mode change. Modbus RTU handles one request at a time, so the order of polling decides which value is fresh at any moment, and the original polling order had not been optimized.
  **Solution:** Reordered the Modbus polling sequence in the S7-200 SMART ladder program. The Auto/Manual setting, control output, and set value are now polled first, and MV Read and Write follow after them. This way, MV is always handled with the current mode and setpoint already in place.
  **Result:** MV Read and Write match after repeated Auto/Manual switching, with N test cycles and no mismatch.

### Roadmap
- Complete runtime data logging (VBScript) and automatic Conditional Formatting.
- Centralize the logged data for analysis and maintenance planning.
