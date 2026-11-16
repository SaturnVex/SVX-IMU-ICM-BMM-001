# ICM-45686 IMU (SPI) Test

**Module:** `SVX-IMU-ICM-BMM-001` · **Sensor:** TDK InvenSense ICM-45686 six-axis IMU · **Host:** `SVX-MCU-STM32G474-001`

Verifies the ICM-45686 on this module: SPI communication, chip identification (WHO_AM_I = 0xE9), initialization, the 3200 Hz data-ready interrupt on INT1, INT2 activity and live accelerometer / gyroscope data.

## Reference code

The communication code is the SaturnVex STM32G474 ICM-45686 example. Use its guide and STM32CubeIDE project without changes:

- [SVX-MCU-STM32G474-001 · Test 04 — ICM-45686 SPI3](https://github.com/SaturnVex/SVX-MCU-STM32G474-001/tree/main/tests/04-icm45686-spi3)
- [Test guide (PDF)](https://github.com/SaturnVex/SVX-MCU-STM32G474-001/blob/main/tests/04-icm45686-spi3/SaturnVex_STM32G474_ICM45686_Test_Guide_EN.pdf)
- [STM32CubeIDE source project (ZIP)](https://github.com/SaturnVex/SVX-MCU-STM32G474-001/raw/main/tests/04-icm45686-spi3/STM32G474_ICM45686_SPI3_SaturnVex.zip)

The project uses the TDK InvenSense ICM-45686 driver with a HAL SPI transport (`icm45686.cpp`), checks WHO_AM_I, estimates the gyro bias at start-up and reads the sensor on every INT1 data-ready interrupt.

## Connections

| SVX-IMU-ICM-BMM-001 H1 pin | Signal | SVX-MCU-STM32G474-001 |
|---|---|---|
| 9 | SCL_SCK (SPI SCK) | SPI3_SCK — PC10 |
| 5 | SDO (SPI MISO) | SPI3_MISO — PC11 |
| 7 | SDA_SDI (SPI MOSI) | SPI3_MOSI — PC12 |
| 11 | CS (active low) | PD2 |
| 6 | INT1 (data ready) | PA15 (IO_2) |
| 8 | INT2 | PB5 (IO_3) |
| 1 / 2 | GND | GND |
| 3 | VIN, 3.3–5.5 V | 3.3 V or 5 V supply |

- All signals are 3.3 V logic; CS, SDI, SCK, INT1 and INT2 have 10 kΩ pull-ups on the module.
- The BMM350 pins (10 / 12 / 14) can stay connected for [Test 02](../02-bmm350-i2c/README.md).

## Run this test

1. Power off both boards and make the connections above with short wires.
2. Import, build and flash the STM32G474 ICM-45686 project as described in its guide.
3. Open a 115200-baud terminal on the STM32G474 USART2 (PB3 TX). Keep the module still during the gyro bias estimation at start-up.
4. Check the output against the acceptance criteria below.

## Acceptance

The terminal shows `=== STM32G474 ICM45686 SPI3 ===`, WHO_AM_I 0xE9, `ICM init rc=0` and `ICM DRDY INT1 enabled, 3200 Hz target`, then once per second `[ICM] INT1=... Hz INT2=... Hz A_milli=... G_milli=...`.

**PASS:** correct WHO_AM_I and `init rc=0`, INT1 close to 3200 Hz, acceleration and gyro values that follow the module orientation and movement, and LED1 / LED2 alternating once per second. The readings are diagnostic values, not a factory calibration.

The STM32G474 project keeps its own licenses: SaturnVex files under BSD-3-Clause, the TDK InvenSense driver, STMicroelectronics HAL and Arm CMSIS under their own terms.

[← All tests](../README.md) · [← Product home](../../README.md)
