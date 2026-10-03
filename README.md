# 🚗 CAN-Driven Vehicle Monitoring and Driver Assistance System

<p align="center">

### 📡 A CAN-Based Multi-Node Vehicle Monitoring and Driver Assistance System

</p>

<p align="center">

<img src="https://img.shields.io/badge/Language-Embedded%20C-blue?style=for-the-badge&logo=c">

<img src="https://img.shields.io/badge/MCU-LPC2129-green?style=for-the-badge">

<img src="https://img.shields.io/badge/Protocol-CAN-orange?style=for-the-badge">

<img src="https://img.shields.io/badge/Transceiver-MCP2551-red?style=for-the-badge">

<img src="https://img.shields.io/badge/IDE-Keil%20%C2%B5Vision-purple?style=for-the-badge">

</p>

---

## 📌 Overview

The **CAN-Driven Vehicle Monitoring and Driver Assistance System** is a distributed embedded system designed to monitor important vehicle parameters and provide driver assistance features using **CAN communication**.

The system is developed using the **LPC2129 ARM7 microcontroller** and consists of three independent nodes connected through a CAN communication network:

- 🖥️ **Main Node**
- ⛽ **Fuel Node**
- 🚨 **Indicator & Reverse Alert Node**

The **Main Node** acts as the central monitoring and control unit. It monitors engine temperature, receives fuel percentage from the Fuel Node, manages Forward/Reverse mode selection, controls indicator commands, and displays vehicle information and reverse-alert status on a 20×4 LCD.

The **Fuel Node** reads the fuel input using the LPC2129 ADC, converts the ADC value into fuel percentage, and transmits the information to the Main Node through CAN.

The **Indicator & Reverse Alert Node** controls the left and right indicators in Forward Mode and performs obstacle detection using the HC-SR04 ultrasonic sensor in Reverse Mode.

---

## 🎯 Aim

To design and implement a **CAN-based multi-node vehicle monitoring and driver assistance system** capable of:

- Monitoring fuel level
- Monitoring engine temperature
- Providing reverse obstacle detection
- Controlling left and right indicators
- Generating driver alerts
- Displaying vehicle information on an LCD
- Communicating between multiple embedded nodes using CAN

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🧠 Multi-Node Architecture | Three LPC2129-based nodes communicate through CAN |
| 📡 CAN Communication | Communication between multiple vehicle nodes |
| ⛽ Fuel Monitoring | Fuel level measurement using ADC |
| 🌡️ Temperature Monitoring | Engine temperature monitoring using DS18B20 |
| 🔄 Forward / Reverse Mode | Vehicle mode selection using external interrupt |
| ↔️ Indicator Control | Left and right indicator control |
| 📏 Reverse Obstacle Detection | HC-SR04 based obstacle detection |
| 🟢 SAFE Status | Indicates obstacle is beyond warning range |
| 🟡 WARNING Status | Generates intermittent buzzer alert |
| 🔴 STOP Status | Generates continuous buzzer and activates alert LED |
| ⚠️ Sensor Fault | Detects missing ultrasonic echo |
| 🖥️ LCD Dashboard | Displays temperature, fuel, mode and reverse status |

---

# 🏗️ System Architecture

The system consists of three LPC2129-based embedded nodes connected through a common **CAN communication network**.

```mermaid
flowchart LR

    FUEL["⛽ FUEL NODE<br/><br/>LPC2129<br/><br/>Fuel Gauge<br/>ADC<br/><br/>Fuel Percentage"]

    MAIN["🖥️ MAIN NODE<br/><br/>LPC2129<br/><br/>20×4 LCD<br/>DS18B20<br/>External Interrupts<br/>Vehicle Mode Control"]

    ALERT["🚨 INDICATOR &<br/>REVERSE ALERT NODE<br/><br/>LPC2129<br/><br/>Indicator LEDs<br/>HC-SR04<br/>Buzzer<br/>Reverse Alert LED"]

    FUEL -->|"CAN ID 0x100<br/>Fuel Percentage"| MAIN

    MAIN -->|"CAN ID 0x200<br/>Mode / Indicator Command"| ALERT

    ALERT -->|"CAN ID 0x300<br/>Distance / Reverse Status"| MAIN

    classDef fuel fill:#D5F5E3,stroke:#27AE60,stroke-width:3px,color:#145A32;
    classDef main fill:#D6EAF8,stroke:#2874A6,stroke-width:3px,color:#154360;
    classDef alert fill:#FADBD8,stroke:#CB4335,stroke-width:3px,color:#78281F;

    class FUEL fuel;
    class MAIN main;
    class ALERT alert;

;


