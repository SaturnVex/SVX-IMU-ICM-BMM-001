# Getting Started

The module has no microcontroller: a host MCU reads the ICM-45686 (6-axis IMU) and the BMM350 (3-axis magnetometer) and runs any sensor fusion.

1. **Power.** Apply 3.3–5.5 VDC to VIN (H1 pin 3) and GND (pins 1 / 2). Onboard regulators supply 3.3 V to the ICM-45686 and the I/O, and 1.8 V to the BMM350 core.
2. **Use 3.3 V logic.** All H1 signals are 3.3 V. CS, SDA_SDI, SCL_SCK, INT1 and INT2 have 10 kΩ pull-ups; BMM_SCK, BMM_SDA and BMM_INT have 1 kΩ pull-ups.
3. **Choose the IMU interface.** SPI: drive CS from the host (active low) and use SCK / SDI / SDO. I²C: leave CS high (pulled up) and use SCL / SDA; SDO selects the I²C address (0x68 low, 0x69 high).
4. **Magnetometer.** The BMM350 has its own I²C bus on pins 10 / 12 with interrupt on pin 14; ADSEL is tied to GND, so its 7-bit address is **0x14**.
5. **Run the examples.** The [test projects](../tests/README.md) use the SaturnVex STM32G474 board as host.

## H1 pinout (2 × 7, 2.54 mm)

| H1 pin | Signal | H1 pin | Signal |
|---|---|---|---|
| 1 | GND | 2 | GND |
| 3 | VIN (3.3–5.5 VDC) | 4 | — (not connected) |
| 5 | SDO — ICM-45686 SPI MISO / I²C address select | 6 | INT1 — ICM-45686 interrupt 1 |
| 7 | SDA_SDI — ICM-45686 I²C SDA / SPI MOSI | 8 | INT2 — ICM-45686 interrupt 2 / FSYNC |
| 9 | SCL_SCK — ICM-45686 I²C SCL / SPI SCK | 10 | BMM_SCK — BMM350 I²C SCL |
| 11 | CS — ICM-45686 SPI chip select | 12 | BMM_SDA — BMM350 I²C SDA |
| 13 | — (not connected) | 14 | BMM_INT — BMM350 interrupt |

Sensor axes are printed on the PCB next to U1 (ICM-45686) and U2 (BMM350).

[← Product home](../README.md)
