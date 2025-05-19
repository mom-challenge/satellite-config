# System Verification Report - SatAlphaV1

**Date:** $(date +'%Y-%m-%d')
**Version:** 1.0

## 1. System Configuration Summary

- **System ID:** SatAlphaV1
- **Description:** Configuration for Satellite Alpha, Version 1
- **Components List:**
    - MainThruster001 (T001): 1 unit(s) at 150.5 kg/unit
    - SolarPanel_A (SP001A): 1 unit(s) at 75.2 kg/unit
    - SolarPanel_B (SP001B): 1 unit(s) at 75.2 kg/unit
    - CommAntenna001 (ANT001): 1 unit(s) at 20.0 kg/unit
    - CommAntenna002 (ANT002): 1 unit(s) at 20.0 kg/unit
- **Calculated Total Mass:** 340.9 kg

## 2. Requirements Verification

| Requirement ID | Description                                                              | Parameter                  | Constraint | Actual Value | Unit | Status    |
|----------------|--------------------------------------------------------------------------|----------------------------|------------|--------------|------|-----------|
| REQ001         | Total system mass for SatAlphaV1 configuration must be acceptable        | calculated_total_mass_kg   | <= 350     | 340.9        | kg   | SATISFIED |
| REQ002         | Maximum individual component mass                                        | component_mass_max         | <= 200     | 150.5        | kg   | SATISFIED |

*(Note: Actual value for REQ002 refers to the heaviest component, MainThruster001)*

## 3. Conclusion

All checked requirements for the `SatAlphaV1` system configuration have been **SATISFIED** based on the current data in `system_config.json` and `requirements.csv`. 