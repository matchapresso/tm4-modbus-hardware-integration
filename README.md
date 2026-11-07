### Temperature Control Interface and Modbus RTU Polling Optimization for TM4 Controllers

### Overview

Temperature sensors (RTD) on this line were hard to maintain because they were not integrated with the PLC or the HMI: no central view, and no logged history to diagnose problems. This project connects TM4 temperature controllers over Modbus RTU, gives operators a clear control interface, and adds the data logging needed to move up to a higher SCADA level.

### System architecture
```RTD sensors → TM4 controllers → Modbus RTU (RS-485) → Siemens S7-200 SMART → WinCC Explorer and TIA Portal V16 with WincCC Runtime Advanced for report formatting.```

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
  After deployment, the displayed MV did not match the written MV right after a mode change. Modbus RTU is a serial, one-request-at-a-time protocol, so data speed and consistency depend on the order of polling, which had not been optimized.
  **Solution:** Reordered the poll list so the mode register and MV are handled in a defined sequence, read MV back only after the write is confirmed, grouped contiguous registers into single requests.
  **Result:** All steps are on trial stages.

### Roadmap
- Complete runtime data logging (VBScript) and automatic Conditional Formatting.
- Centralize the logged data for analysis and maintenance planning.
