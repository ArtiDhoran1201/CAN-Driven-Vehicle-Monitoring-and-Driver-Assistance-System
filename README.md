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

The **Main Node** acts as the central monitoring and control unit. It monitors engine temperature, receives fuel percentage from the Fuel Node, manages Forward/Reverse mode selection, controls indicator commands, and displays vehicle information and reverse-alert status on a **20×4 LCD**.

The **Fuel Node** reads the fuel input using the LPC2129 ADC, converts the ADC value into fuel percentage, and transmits the information to the Main Node through CAN.

The **Indicator & Reverse Alert Node** controls the left and right indicators in Forward Mode and performs obstacle detection using the **HC-SR04 ultrasonic sensor** in Reverse Mode.

---

## 🎯 Aim

To design and implement a **CAN-based multi-node vehicle monitoring and driver assistance system** capable of:

- ⛽ Monitoring fuel level
- 🌡️ Monitoring engine temperature
- 📏 Providing reverse obstacle detection
- ↔️ Controlling left and right indicators
- 🔔 Generating driver alerts
- 🖥️ Displaying vehicle information on an LCD
- 📡 Communicating between multiple embedded nodes using CAN

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

```text
                    🚗 VEHICLE MONITORING SYSTEM
                              │
                    ┌─────────┴─────────┐
                    │                   │
              📡 CAN COMMUNICATION NETWORK
                    │
        ┌───────────┼───────────────┐
        │           │               │
        ↓           ↓               ↓

┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐
│ ⛽ FUEL NODE  │  │ 🖥️ MAIN NODE │  │ 🚨 INDICATOR &       │
│              │  │              │  │ REVERSE ALERT NODE   │
│  LPC2129     │  │  LPC2129     │  │      LPC2129         │
│              │  │              │  │                      │
│ Fuel Sensor  │  │ DS18B20      │  │ Left Indicator       │
│ ADC          │  │ 20×4 LCD     │  │ Right Indicator      │
│              │  │ Switches     │  │ HC-SR04              │
│ Fuel %       │  │ Mode Control │  │ Buzzer               │
│              │  │              │  │ Alert LED            │
└──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘
       │                  │                     │
       └──────────────────┼─────────────────────┘
                          │
                     📡 CAN BUS
BUS
📡 CAN Communication Architecture

The system uses a CAN-based multi-node architecture in which three LPC2129-based nodes communicate with each other through the CAN network.

🔹 Nodes in the System
⛽ Fuel Node – Measures fuel level using ADC and sends fuel percentage to the Main Node.
🖥️ Main Node – Acts as the central monitoring and control unit.
🚨 Indicator & Reverse Alert Node – Controls indicators and performs reverse obstacle detection.
🔹 CAN Communication Flow
┌──────────────────────┐
│      ⛽ FUEL NODE     │
│       LPC2129        │
│                      │
│ Fuel Sensor → ADC    │
│        ↓             │
│ Fuel Percentage      │
└──────────┬───────────┘
           │
           │ CAN ID: 0x100
           │ Fuel Percentage
           ↓
════════════════════════════
          CAN BUS
════════════════════════════
           │
           ↓
┌──────────────────────┐
│     🖥️ MAIN NODE      │
│       LPC2129        │
│                      │
│ DS18B20 Temperature  │
│ Mode Selection       │
│ 20×4 LCD             │
└──────────┬───────────┘
           │
           │ CAN ID: 0x200
           │ Mode / Indicator
           │ Command
           ↓
┌────────────────────────────┐
│ 🚨 INDICATOR & REVERSE NODE│
│           LPC2129          │
│                            │
│ Left / Right Indicators    │
│ HC-SR04 Ultrasonic Sensor  │
│ Buzzer + Alert LED         │
└────────────┬───────────────┘
             │
             │ CAN ID: 0x300
             │ Distance /
             │ Reverse Status
             ↓
        🖥️ MAIN NODE
🔄 CAN Data Flow

The CAN communication between the three nodes is performed using different CAN message IDs.

⛽ Fuel Node → Main Node
Fuel Sensor
     ↓
    ADC
     ↓
Fuel Percentage
     ↓
CAN ID 0x100
     ↓
Main Node
     ↓
20×4 LCD

The Fuel Node measures the fuel level using the ADC, converts the ADC value into fuel percentage and transmits the result to the Main Node using CAN ID 0x100.

🖥️ Main Node → Indicator & Reverse Alert Node
Mode Selection
      ↓
Forward / Reverse
      ↓
Indicator Command
      ↓
CAN ID 0x200
      ↓
Indicator & Reverse Alert Node

The Main Node sends the selected vehicle mode and indicator control commands to the Indicator & Reverse Alert Node using CAN ID 0x200.

🚨 Indicator & Reverse Alert Node → Main Node
HC-SR04
   ↓
Distance Measurement
   ↓
SAFE / WARNING / STOP
   ↓
CAN ID 0x300
   ↓
Main Node
   ↓
LCD Display

The Indicator & Reverse Alert Node measures the obstacle distance during Reverse Mode and sends the distance and reverse status to the Main Node using CAN ID 0x300.

🖥️ Main Node

The Main Node is the central monitoring and control unit of the system.

Responsibilities
🌡️ Monitor engine temperature using DS18B20.
⛽ Receive fuel percentage from the Fuel Node.
🔄 Manage Forward and Reverse Mode.
↔️ Control left and right indicator commands.
📡 Communicate with other nodes through CAN.
📺 Display vehicle information on the 20×4 LCD.
🚨 Display reverse obstacle status received from the Indicator & Reverse Alert Node.
Main Node Flow
                    MAIN NODE
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ↓             ↓             ↓
     DS18B20       CAN Receive     Switches
   Temperature      Fuel Data      Mode / Indicator
          │             │             │
          └─────────────┼─────────────┘
                        ↓
                    LPC2129
                        ↓
                    20×4 LCD
                        ↓
              Vehicle Information
⛽ Fuel Node

The Fuel Node is responsible for measuring and transmitting the vehicle fuel level.

Working
Fuel Sensor
     ↓
ADC Input
     ↓
LPC2129 ADC
     ↓
ADC Value
     ↓
Fuel Percentage
     ↓
CAN ID 0x100
     ↓
Main Node
Fuel Percentage Calculation
Fuel Percentage = (ADC Value × 100) / 1023

The calculated fuel percentage is transmitted to the Main Node through CAN.

🚨 Indicator & Reverse Alert Node

This node performs two main functions:

↔️ Indicator control during Forward Mode
📏 Reverse obstacle detection during Reverse Mode
🟢 Forward Mode

When the vehicle is in Forward Mode:

Forward Mode
     ↓
Indicator Switch
     ↓
Left / Right Selection
     ↓
CAN Command
     ↓
Indicator Node
     ↓
Left / Right Indicator

The node controls the left and right indicator LEDs according to the command received from the Main Node.

🔴 Reverse Mode

When Reverse Mode is selected:

Reverse Mode
     ↓
HC-SR04 Enabled
     ↓
Trigger Pulse
     ↓
Echo Measurement
     ↓
Distance Calculation
     ↓
Distance Comparison
     ↓
SAFE / WARNING / STOP
     ↓
CAN ID 0x300
     ↓
Main Node
     ↓
LCD + Alert
📏 Reverse Obstacle Detection

The HC-SR04 ultrasonic sensor is used to measure the distance between the vehicle and the obstacle.

Distance	Status	Buzzer	Alert LED
🟢 > 100 cm	SAFE	OFF	OFF
🟡 41–100 cm	WARNING	Intermittent	OFF
🔴 ≤ 40 cm	STOP	Continuous	ON
🟣 No Echo	SENSOR FAULT	Fault indication	OFF
🟢 SAFE
Distance > 100 cm
       ↓
     SAFE
       ↓
Buzzer OFF
Alert LED OFF
🟡 WARNING
Distance = 41–100 cm
       ↓
    WARNING
       ↓
Intermittent Buzzer
🔴 STOP
Distance ≤ 40 cm
       ↓
      STOP
       ↓
Continuous Buzzer
       +
Reverse Alert LED ON
🟣 Sensor Fault
No Valid Echo
      ↓
SENSOR FAULT
🌡️ Engine Temperature Monitoring

The DS18B20 sensor is used to monitor the engine temperature.

DS18B20
   ↓
Temperature Reading
   ↓
LPC2129
   ↓
20×4 LCD
   ↓
Temperature Display
🖥️ LCD Display

The 20×4 LCD displays important vehicle parameters and system status.

Forward Mode
┌────────────────────┐
│ TEMP: 32°C         │
│ FUEL: 44%          │
│ MODE: FORWARD      │
│ L: LEFT  R: RIGHT  │
└────────────────────┘
Reverse Mode
┌────────────────────┐
│ TEMP: 32°C         │
│ FUEL: 44%          │
│ DIST: 164CM        │
│ SAFE               │
└────────────────────┘
📡 CAN Message Map
CAN ID	Sender	Receiver	Data
0x100	Fuel Node	Main Node	Fuel Percentage
0x200	Main Node	Indicator & Reverse Alert Node	Mode / Indicator Command
0x300	Indicator & Reverse Alert Node	Main Node	Distance / Reverse Status
🔌 Hardware Components
Component	Purpose
LPC2129 ARM7	Main processing and control
MCP2551	CAN transceiver
20×4 LCD	Vehicle information display
DS18B20	Engine temperature monitoring
HC-SR04	Reverse obstacle detection
Fuel Sensor	Fuel-level input
ADC	Fuel measurement
Indicator LEDs	Left and right indicators
Buzzer	Warning and STOP alert
Reverse Alert LED	Obstacle alert
Switches	Mode and indicator control
USB-to-UART Converter	Serial communication / debugging
🔔 External Interrupts

External interrupts are used for vehicle mode and indicator control.

Mode Selection
Mode Switch
     ↓
External Interrupt
     ↓
FORWARD ↔ REVERSE
Left Indicator
Left Indicator Switch
        ↓
External Interrupt
        ↓
Left Indicator Command
Right Indicator
Right Indicator Switch
        ↓
External Interrupt
        ↓
Right Indicator Command
📂 Project Structure
CAN-Driven-Vehicle-Monitoring-and-Driver-Assistance-System/
│
├── Main_Node/
│   ├── inc/
│   │   └── *.h
│   │
│   └── src/
│       └── *.c
│
├── Fuel_Node/
│   ├── inc/
│   │   └── *.h
│   │
│   └── src/
│       └── *.c
│
├── Indicator_Reverse_Alert_Node/
│   ├── inc/
│   │   └── *.h
│   │
│   └── src/
│       └── *.c
│
├── Proteus/
│   └── simulation-files
│
├── Images/
│   ├── hardware-overview.jpg
│   ├── lcd-project-title.jpg
│   ├── forward-mode-output.jpg
│   └── reverse-mode-output.jpg
│
└── README.md
🧪 Proteus Simulation

The system was tested in Proteus to verify the operation of the individual nodes and CAN communication before hardware implementation.

Simulation includes:
LPC2129 microcontrollers
MCP2551 CAN transceivers
CAN communication
20×4 LCD
DS18B20 temperature sensor
ADC-based fuel monitoring
HC-SR04 ultrasonic sensor
Indicator LEDs
Buzzer
External interrupt switches
Proteus Screenshot
<p align="center"> <img src="Images/proteus-simulation.png" alt="Proteus Simulation" width="90%"> </p>
📸 Real Hardware Implementation

The system was implemented and tested on actual LPC2129-based development hardware.

🏗️ Complete Hardware Setup
<p align="center"> <img src="Images/hardware-overview.jpg" alt="Complete Hardware Setup" width="90%"> </p>

The complete hardware setup consists of the LPC2129 development boards, CAN communication circuitry, LCD, sensors, indicator LEDs, buzzer and supporting connections.

📺 Project Title Display
<p align="center"> <img src="Images/lcd-project-title.jpg" alt="Project Title Display" width="75%"> </p>

The LCD displays the project title during system initialization.

🟢 Forward Mode Output
<p align="center"> <img src="Images/forward-mode-output.jpg" alt="Forward Mode Output" width="75%"> </p>

In Forward Mode, the system monitors the vehicle parameters and controls the left and right indicators according to the selected command.

🔴 Reverse Mode Output
<p align="center"> <img src="Images/reverse-mode-output.jpg" alt="Reverse Mode Output" width="75%"> </p>

In Reverse Mode, the HC-SR04 sensor measures the obstacle distance and the system displays the distance and corresponding safety status on the LCD.

🛠️ Development Environment
Tool	Details
IDE	Keil µVision
Microcontroller	LPC2129 ARM7
Programming Language	Embedded C
Communication Protocol	CAN
CAN Transceiver	MCP2551
Simulation Tool	Proteus
Flashing Tool	Flash Magic
⚙️ Complete System Working
                         POWER ON
                            ↓
                  Initialize GPIO
                            ↓
                  Initialize CAN
                            ↓
                  Initialize LCD
                            ↓
                  Initialize Sensors
                            ↓
              Configure External Interrupts
                            ↓
                   Read Temperature
                            ↓
                  Receive Fuel Data
                            ↓
                  Check Vehicle Mode
                            ↓
              ┌─────────────┴─────────────┐
              ↓                           ↓
        FORWARD MODE                 REVERSE MODE
              ↓                           ↓
      Indicator Control              HC-SR04
              ↓                           ↓
        Left / Right                  Distance
         Indicator                  Measurement
              ↓                           ↓
              └─────────────┬─────────────┘
                            ↓
                    CAN Communication
                            ↓
                     Main Node Display
                            ↓
                          LCD
🔄 Overall System Flow
┌──────────────────────┐
│      POWER ON        │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ System Initialization│
│ GPIO / CAN / LCD     │
│ Sensors / Interrupts │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Vehicle Monitoring │
└──────────┬───────────┘
           ↓
     ┌─────┴─────┐
     ↓           ↓
  FORWARD      REVERSE
   MODE          MODE
     ↓           ↓
Indicators    HC-SR04
     ↓           ↓
     │        Distance
     │        Measurement
     │           ↓
     │      SAFE / WARNING
     │          / STOP
     │           ↓
     └─────┬─────┘
           ↓
      CAN Communication
           ↓
       Main Node
           ↓
      20×4 LCD Display
🚗 Applications

The system can be used in:

Automotive monitoring systems
Driver assistance systems
Reverse parking assistance
Vehicle dashboard systems
CAN-based automotive networks
Vehicle parameter monitoring
Distributed automotive control systems
Embedded automotive applications
🔮 Future Enhancements

Possible future enhancements include:

🚘 Vehicle speed monitoring
🔋 Battery voltage monitoring
📊 Advanced dashboard
💾 Vehicle data logging
📍 GPS-based vehicle tracking
📡 Wireless vehicle monitoring
🌐 IoT-based vehicle monitoring
🔧 Additional engine parameter monitoring
⭐ Conclusion

The CAN-Driven Vehicle Monitoring and Driver Assistance System demonstrates a distributed automotive embedded system using multiple LPC2129 nodes communicating through CAN.

The system integrates fuel monitoring, engine temperature monitoring, Forward/Reverse mode selection, indicator control, reverse obstacle detection, buzzer alerts and LCD-based vehicle monitoring.

The project demonstrates practical implementation of Embedded C, LPC2129 ARM7, ADC, external interrupts, CAN communication, MCP2551 interfacing, sensor interfacing, LCD interfacing, Proteus simulation and real hardware implementation
