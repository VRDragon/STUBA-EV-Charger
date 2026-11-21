# Charger_F105
### Embedded Charger Control System – STUBA Green Team

**Charger_F105** is an embedded firmware development project created during my involvement in **STUBA Green Team at the Faculty of Electrical Engineering and Information Technology, Slovak University of Technology in Bratislava (FEI STU)**.

The project focuses on developing a microcontroller-based control system for a charging power supply. It uses an STM32F105 microcontroller to acquire voltage and current measurements, generate analog control signals, monitor operating conditions and display system information.

The firmware was developed in C using the STM32 HAL library and STM32CubeMX configuration tools.

---

## Project Overview

The main objective was to develop an embedded control unit for a charging system, integrating measurement, regulation interfaces, user feedback and safety-related inputs.

The controller provides:
- Measurement of voltage and current.
- Acquisition of analog control inputs.
- Digital-to-analog conversion for voltage and current reference signals.
- DMA-based ADC sampling.
- OLED display for operating information.
- GPIO-based status indication.
- Inputs for overtemperature detection and system blocking.
- CAN peripheral initialization for potential communication with other system components.

The firmware also contains the structure for several operating modes, including manual operation and automatic charging control.

---

## Main Features

### Analog Measurement

The STM32 ADC peripheral is configured to acquire four analog channels:

| ADC Channel | Function |
|---|---|
| ADC1_IN0 | Voltage measurement |
| ADC1_IN1 | Current measurement |
| ADC1_IN2 | Voltage reference / setpoint |
| ADC1_IN3 | Current reference / setpoint |

The ADC operates in continuous scan mode and uses DMA to transfer measurement data into a memory buffer.

### DMA-Based Data Acquisition

The firmware uses a multidimensional buffer:

```c
uint16_t ADC1_BUF[128][4];
```

Measurement data from four ADC channels is collected using DMA.

The function `DMAMeranie()` calculates average values from 126 measurement samples for each channel.

This approach helps reduce measurement fluctuations and provides more stable values for display and control calculations.

### Analog Control Outputs

Two DAC channels are configured to generate analog reference signals.

| DAC Channel | Function |
|---|---|
| DAC_CHANNEL_1 | Voltage reference |
| DAC_CHANNEL_2 | Current reference |

The firmware includes functions for setting reference values:

```c
void nastavU(float napetie);
void nastavI(float prud);
```

It also provides direct binary DAC control:

```c
void nastavUbin(uint32_t Ubinar);
void nastavIbin(uint32_t Ibinar);
```

Calibration constants are used to convert between physical quantities and digital DAC values.

### OLED Display

The project integrates an SSD1306 OLED display using I²C communication.

The display provides information about:

- Voltage setpoint (`Uz`).
- Measured voltage (`Um`).
- Current setpoint (`Iz`).
- Measured current (`Im`).
- IMD and BMS status indicators.

The main display functions include:

```c
void InitVypis();
void vypisOLED();
void BlockVypis();
```

The startup screen displays the project identification:

**NES CHARGER – STUBA GREEN TEAM**

### Safety and Monitoring Inputs

The firmware defines dedicated GPIO signals for system monitoring.

| Signal | STM32 Pin | Function |
|---|---|---|
| blokacia | PA6 | System blocking output |
| prehriatie | PA7 | Overtemperature input |
| PIN1 | PC6 | Digital input |
| PIN2 | PC7 | Digital input |
| PIN3 | PC8 | Digital input |
| PIN4 | PC9 | Digital input |

Five additional GPIO outputs are configured for LED status indication.

### CAN Communication

The CAN1 peripheral is initialized in normal operating mode.

The configuration provides a foundation for communication with other charging-system components.

The repository does not currently contain a complete CAN communication protocol or message-processing implementation.

---

## Operating Modes

The firmware defines five operating modes:

| Mode | Name | Intended Function |
|---|---|---|
| 0 | Startup | Initialization and system locking |
| 1 | Manual mode | Basic manual voltage and current control |
| 2 | Automatic voltage | Automatic voltage regulation |
| 3 | Automatic current | Automatic current regulation using PWM |
| 4 | Fast charging | Fast charging operating mode |

The current implementation primarily contains the basic manual control functionality.

The automatic regulation modes and fast charging mode are represented by function structures but are not fully implemented in the available source code.

---

## System Architecture

The firmware is organized around the STM32 microcontroller and its integrated peripherals.

```text
              STM32F105
                  |
       +----------+----------+
       |          |          |
      ADC         DAC        GPIO
       |          |          |
    4 analog    Voltage    Safety inputs
    channels    reference  and LED outputs
       |        Current
       |        reference
       |
      DMA
       |
   Measurement
   processing
       |
       +----------+
       |          |
     OLED       Control
    SSD1306     functions
       |
     I
