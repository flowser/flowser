# Case study: production campus network and services

**Client:** Chesta Teachers Training College, West Pokot, Kenya · **Year:** 2026 · **Role:** Lead engineer

[← Back to profile](https://github.com/flowser)

---

## The problem

A rural college needed reliable internet for teaching and administration, a way to lock down the network during national exams without cutting off staff, and its own software platform, all in a location where a single ISP outage used to take the whole campus offline.

## What I delivered

### Resilient connectivity
- **Dual-WAN failover:** Starlink as primary, Airtel as automatic failover, on a virtualised MikroTik router (CHR). Teaching and administration survive a single-ISP failure.
- **Separated networks:** student and staff traffic run on separate network paths, so locking down students during an exam never silences the registrar or the principal's office.
- **Campus DNS** (BIND) for internal services.

### Identity-based Wi-Fi
- **FreeRADIUS and UniFi** enterprise Wi-Fi: every student signs in with their own college credentials, imported from the roster, instead of a shared password.
- Per-user device limits, and the ability to disconnect and re-authenticate users during exam week.

### Exam lockdown
- One switch puts the campus into **exam mode**: student internet is blocked, approved national exam servers stay reachable, device limits are enforced, and staff connectivity is unaffected.

### Self-hosted services on Proxmox
- The **AEMMS** education platform (see the [AEMMS case study](aemms-platform.md)), deployed on campus with desktop clients packaged for the college.
- **Mailcow** email and **Jitsi Meet** video conferencing, hosted on-site.
- **NetworkMate**, a web control room for the IT team to manage students, devices and exam mode without editing router configs.

### Handover
- Equipment inventory and operations runbooks so the college's own team can run and restart everything.
- A fibre modernisation proposal and bill of quantities for the next phase.

## Architecture

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart TB
  SL["🛰️ Starlink<br/>primary WAN"]:::wan
  AT["📶 Airtel<br/>failover WAN"]:::wan
  GW{{"🧭 MikroTik CHR gateway<br/>dual-WAN · firewall · exam mode"}}:::core
  SL ==> GW
  AT -.->|if Starlink drops| GW
  subgraph lan["🏫 Campus LAN"]
    direction TB
    ST["🎓 Student network<br/>identity Wi-Fi"]:::trainee
    SF["🧑‍💼 Staff network<br/>online during exams"]:::staff
    RAD["🔐 FreeRADIUS + UniFi<br/>per-student login · device limits"]:::net
    ST <--> RAD
  end
  subgraph px["🗄️ Proxmox VE host"]
    direction TB
    NM["🛡️ NetworkMate<br/>control room"]:::net
    AE["🏫 AEMMS"]:::ai
    MC["📧 Mailcow"]:::data
    JT["🎥 Jitsi Meet"]:::data
  end
  GW --> ST
  GW --> SF
  GW -.-|RouterOS API · exam mode| NM
  GW --> AE
  GW --> MC
  GW --> JT
  classDef wan fill:#b45309,stroke:#fcd34d,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef net fill:#0d9488,stroke:#5eead4,stroke-width:2px,color:#ffffff
  classDef ai fill:#9333ea,stroke:#d8b4fe,stroke-width:2px,color:#ffffff
  classDef data fill:#1e1b4b,stroke:#818cf8,stroke-width:2px,color:#e0e7ff
  style lan fill:#0d0d1a,stroke:#0284c7,stroke-width:2px,color:#7dd3fc
  style px fill:#0d0d1a,stroke:#9333ea,stroke-width:2px,color:#d8b4fe
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

## Exam mode, step by step

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','actorBkg':'#4f46e5','actorBorder':'#a5b4fc','actorTextColor':'#ffffff','actorLineColor':'#6b6b8a','signalColor':'#0ea5e9','signalTextColor':'#0ea5e9','labelBoxBkgColor':'#f97316','labelBoxBorderColor':'#fdba74','labelTextColor':'#ffffff','loopTextColor':'#f97316','noteBkgColor':'#16162a','noteTextColor':'#f0f0ff','noteBorderColor':'#f97316','activationBkgColor':'#0ea5e9','activationBorderColor':'#7dd3fc','sequenceNumberColor':'#ffffff'}}}%%
sequenceDiagram
  autonumber
  participant IT as 🧑‍💻 IT officer
  participant NM as 🛡️ NetworkMate
  participant GW as 🧭 MikroTik gateway
  participant ST as 🎓 Student devices
  participant SF as 🧑‍💼 Staff devices
  rect rgba(234, 88, 12, 0.16)
    Note over IT,GW: Exam starts
    IT->>NM: Turn exam mode on
    NM->>GW: Enable exam firewall rules (RouterOS API)
    NM->>GW: Allow each student's primary device only
    GW-->>ST: Internet blocked, exam hosts allowed
    GW-->>SF: No change, staff stay online
  end
  rect rgba(5, 150, 105, 0.16)
    Note over IT,GW: Exam ends
    IT->>NM: Turn exam mode off
    NM->>GW: Disable exam firewall rules
    GW-->>ST: Normal access restored
  end
```

## WAN failover

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#0d9488','primaryTextColor':'#ffffff','primaryBorderColor':'#5eead4','lineColor':'#0ea5e9','edgeLabelBackground':'#16162a','transitionColor':'#0ea5e9','transitionLabelColor':'#0ea5e9','stateLabelColor':'#ffffff','labelColor':'#ffffff'}}}%%
stateDiagram-v2
  direction LR
  [*] --> Starlink
  Starlink: 🛰️ Starlink active
  Airtel: 📶 Airtel active
  Starlink --> Airtel: health check fails
  Airtel --> Starlink: Starlink recovers
  classDef primary fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef backup fill:#b45309,stroke:#fcd34d,stroke-width:2px,color:#ffffff
  class Starlink primary
  class Airtel backup
```

## Stack

MikroTik RouterOS / CHR · FreeRADIUS · UniFi OS · Proxmox VE · BIND · Docker · Mailcow · Jitsi · Django · Vue

---

**Need a campus or office network designed and deployed?** [Email me](mailto:eng.felixnyachio@gmail.com?subject=Network%20project%20inquiry) · [WhatsApp](https://wa.me/254748650011)
