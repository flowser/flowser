# Case study: embedded, IoT and field engineering

**Role:** Firmware, hardware and full-stack engineer · **Period:** 2015 to now

[← Back to profile](https://github.com/flowser)

---

I'm an Electrical & Electronic Engineer, so a lot of my work starts at the circuit board and ends at a web dashboard. These are some of the projects where hardware, firmware and software had to work together on real sites.

## PAYGO e-bikes and battery management

**Problem:** electric bikes sold on pay-as-you-go plans need to know, in real time, whether the rider has paid, and batteries need protecting from abuse to last their full life.

**What I built:**
- **Firmware** on STM32 in embedded C, controlling the bike according to payment status.
- **Battery management telemetry:** individual cell data streamed to the cloud and stored in MySQL, with alerts and feedback to the motor controller.
- **Backend and dashboard** in Laravel and Vue, showing payment status, battery health and alerts.

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart LR
  subgraph vehicle["🛵 On the vehicle"]
    CELLS["🔋 Battery cells"]:::hw
    BMS["BMS board<br/>cell sensing"]:::hw
    MCU["🧩 STM32 firmware<br/>embedded C"]:::staff
    MOT["⚙️ Motor controller"]:::hw
  end
  subgraph cloud["☁️ Cloud"]
    API{{"Laravel IoT API<br/>MySQL"}}:::core
    DASH["📊 Vue dashboard<br/>alerts · fleet view"]:::trainee
    PAY["💳 Payment status<br/>pay-as-you-go"]:::cloud
  end
  CELLS --> BMS --> MCU
  MCU <-->|feedback| MOT
  MCU ==>|cell telemetry| API
  API --> DASH
  PAY --> API
  API -.->|enable / disable| MCU
  classDef hw fill:#b45309,stroke:#fcd34d,stroke-width:2px,color:#ffffff
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  classDef cloud fill:#db2777,stroke:#f9a8d4,stroke-width:2px,color:#ffffff
  style vehicle fill:#1a1206,stroke:#f59e0b,stroke-width:2px,color:#fcd34d
  style cloud fill:#0d0d1a,stroke:#4f46e5,stroke-width:2px,color:#7dd3fc
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

**Stack:** STM32 · embedded C · Laravel · Vue · MySQL · IoT

## Ventilator reverse engineering

**Problem:** demonstrate that a low-cost mechanical ventilator is feasible, with remote monitoring.

**What I built:** reverse-engineered a mechanical ventilator and built a remote-monitoring web platform for it, plus firmware for real-time temperature and pressure measurement on medical equipment, with live graphs.

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart LR
  subgraph device["🫁 Ventilator prototype"]
    SEN["🌡️ Temperature and<br/>pressure sensors"]:::hw
    FW["🧩 Real-time firmware"]:::staff
    ACT["⚙️ Mechanical ventilator"]:::hw
  end
  WEB{{"🌐 Remote monitoring<br/>web platform"}}:::core
  UI["📈 Live graphs<br/>remote view"]:::trainee
  SEN --> FW
  FW <--> ACT
  FW ==>|live readings| WEB
  WEB --> UI
  classDef hw fill:#b45309,stroke:#fcd34d,stroke-width:2px,color:#ffffff
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  style device fill:#1a1206,stroke:#f59e0b,stroke-width:2px,color:#fcd34d
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

**Stack:** firmware · sensors · web platform · real-time charts

## Smart home control

A home automation system running in a lived-in home: lights and appliances controlled from an Android app and the web, over MQTT.

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart LR
  APP["📱 Android app"]:::trainee
  WEBUI["🌐 Web dashboard"]:::trainee
  DJ{{"⚙️ Django server"}}:::core
  PI["🍓 Raspberry Pi hub"]:::hw
  BRK["📡 MQTT broker"]:::net
  subgraph house["🏠 In the home"]
    N1["💡 Lighting node<br/>Arduino"]:::hw
    N2["🔌 Appliance node<br/>Arduino"]:::hw
  end
  APP --> DJ
  WEBUI --> DJ
  DJ <--> BRK
  PI <--> BRK
  BRK <--> N1
  BRK <--> N2
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef net fill:#0d9488,stroke:#5eead4,stroke-width:2px,color:#ffffff
  classDef hw fill:#b45309,stroke:#fcd34d,stroke-width:2px,color:#ffffff
  style house fill:#1a1206,stroke:#f59e0b,stroke-width:2px,color:#fcd34d
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

**Stack:** Arduino · Raspberry Pi · Django · Android · MQTT

## Vending and mobile money

Vending machine controllers integrated with M-Pesa mobile-money payments.

## Power, solar and industrial field service

Contract engineering in 2022 for Gaviton Enterprises, C.P. Power EA, Neural Power, Konza Elevators and Latenight Dentist:

- **Solar and power:** SMA inverter commissioning, control-PCB troubleshooting, UPS systems and generators.
- **Plant:** motors, relays, overhead cranes, borehole and dosing pumps, reverse-osmosis water systems, with spare-parts inventory systems.
- **Vertical transport:** lift LED displays, industrial motherboards and escalator reprogramming on live sites.
- **Medical equipment:** hydraulics and regulator repair on autonomous dental chairs.

## Tools

C / C++ · STM32CubeIDE · Arduino · Raspberry Pi · KiCad (PCB) · PLC · MQTT · SolidWorks · AutoCAD

---

**Need firmware, a connected device or a hardware prototype?** [Email me](mailto:eng.felixnyachio@gmail.com?subject=Embedded%20project%20inquiry) · [WhatsApp](https://wa.me/254748650011)
