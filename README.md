# Edge AI Based Smart EV Charging Station Optimizer

A multi-bay EV charging station simulation where each bay senses its own electrical load, runs a small Edge AI model on an ESP32, and decides locally whether to **ALLOW**, **THROTTLE**, or **DEFER** charging. Data flows over MQTT to a ThingsBoard dashboard for live monitoring and remote control.

Built as part of my IoT internship at Emertxe Information Technologies and as my final-year ECE project.

![Bay 1 dashboard](docs/dashboard-bay1.png)

## Why this project

When several EVs charge at the same time, the combined power draw can overload the station. Instead of sending everything to the cloud and waiting for a response, each bay makes its own decision on the device (Edge AI) and the cloud is used for monitoring, alarms, and manual override.

## How it works

1. Each bay (ESP32) senses voltage, current, power, and temperature.
2. An on-device model predicts the charging session behaviour (such as remaining duration).
3. The optimization logic compares the load against the limit and picks a decision: **ALLOW**, **THROTTLE** (reduce charging level), or **DEFER** (hold the session).
4. Telemetry and decisions are published over **MQTT** to **ThingsBoard**.
5. The dashboard shows live values and lets an operator send commands (set throttle, turn the relay on or off) through RPC.

```
 Sensors -> ESP32 (Edge AI + optimization) --MQTT--> ThingsBoard Cloud
                  ^                                        |
                  +---------- RPC commands (throttle, relay)+
```

## Features

- Multi-bay charging simulation (3 bays)
- On-device prediction and load-based decision making
- Live ThingsBoard dashboard per bay: power, voltage, current, temperature, throttle level, decision, session prediction, overload status
- Alarms for overload conditions
- Remote control with RPC: set throttle level, turn relay on or off
- Device attributes for status tracking

## Results

> Replace with your real numbers.

- Peak power draw: **[X] W managed vs [Y] W unmanaged baseline ([Z]% reduction)**
- Measured over: **[test setup / duration]**

## Tech stack

| Area | Tools |
|---|---|
| Microcontroller | ESP32 |
| Firmware | Embedded C/C++, PlatformIO, Arduino framework |
| Simulation | Wokwi |
| Messaging | MQTT |
| Cloud and dashboard | ThingsBoard (dashboards, rule chains, alarms, RPC) |
| Edge AI | On-device model (training scripts in `edge_ai/`) |

## Repository structure

> Adjust folder names to match your actual repo.

```
.
├── firmware/
│   └── Bay2/                # PlatformIO project (repeat per bay)
│       ├── src/main.cpp
│       └── include/         # State, config, Peripherals, Network, Telemetry,
│                            # model, edge_ai, optimization, rpc, attributes
├── edge_ai/                 # Model training scripts
├── thingsboard/             # Dashboard, rule chain, and alarm configuration
├── docs/                    # SRS/SDD, screenshots, demo links
└── README.md
```

## Getting started

### Prerequisites

- [PlatformIO](https://platformio.org/) (VS Code extension or CLI)
- A [ThingsBoard Cloud](https://thingsboard.cloud/) account (free tier works)
- Wokwi account or an ESP32 board

### Setup

1. Clone the repository
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>/firmware/Bay2
   ```
2. In ThingsBoard, create a device for each bay and copy its access token.
3. Open `include/config.h` and set your Wi-Fi details, MQTT broker, and device token. Do not commit real credentials.
4. Build and run
   ```bash
   pio run
   ```
5. Run it in Wokwi or upload to an ESP32.
6. Import the dashboard and rule chain from `thingsboard/` into your ThingsBoard account.

## Demo

- Demo video: (https://youtu.be/diuMlX-sst8)


## What I learned

- Structuring embedded firmware into separate modules (state, networking, telemetry, optimization, RPC)
- Publishing telemetry over MQTT and handling remote commands on the device
- Building ThingsBoard dashboards, alarms, and rule chains
- Making local decisions on the device instead of depending on the cloud

## Future work

- Run on real ESP32 hardware with physical current and voltage sensors
- Add OTA firmware updates
- Improve the prediction model with more training data

## Author

**Rishikesh Achari**
Final-year ECE student, JNTUA, Anantapur
[LinkedIn](https://www.linkedin.com/in/rishikesh-achari) | [Portfolio](https://rishikeshachari.github.io/Rishi-Portfolio/)

## Acknowledgements

Developed during my internship at Emertxe Information Technologies.
