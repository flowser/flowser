# Case study: AEMMS, an education management platform

**Role:** Founder, architect and lead engineer · **Status:** in production · **Code:** private (walkthrough available on request)

[← Back to profile](https://github.com/flowser)

---

## The problem

Colleges ran exams, marking, results and field supervision on paper, spreadsheets and rented cloud tools. Exam mornings depended on the public internet, student data sat on third-party servers, and every department used a different system.

The client needed **one platform the institution owns**: it runs on campus hardware, keeps working when the internet drops, and handles the full academic cycle from lesson planning to transcripts.

## What I built

A suite of connected products, all designed and built end to end:

| Product | Users | What it does |
|---|---|---|
| **SystemMate** | Staff | Desktop app for lessons, LMS content, assessment authoring (including maths notation), marking, results release, timetables and field-supervision boards |
| **ExamMate** | Students | Locked-down desktop client for sitting exams, reading lessons and viewing results |
| **Field app** | Supervisors | Offline-first mobile app for school visits and industrial-attachment assessment, with rubrics, photos and sync when back online |
| **Core API** | All clients | Multi-tenant institution backend with authentication, background jobs, device workers and real-time events |
| **AI service** | Staff and students | On-premises face verification, retrieval-augmented learning assistant over course material, and task agents |
| **Vendor server** | Savvytex | Issues cryptographically signed licences to each campus deployment; company CMS and sync ledger |

## Architecture

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart TB
  subgraph vendor["☁️ Savvytex cloud"]
    VS["🔑 Vendor control plane<br/>plans · releases · Ed25519 licences"]:::cloud
  end
  subgraph campus["🏫 Campus network · data stays on site"]
    SM["🖥️ SystemMate<br/>staff desktop"]:::staff
    EM["🔒 ExamMate<br/>trainee exam client"]:::trainee
    PF["📱 Field app<br/>offline-first mobile"]:::field
    API{{"⚙️ Institution API<br/>Django REST · JWT · Celery"}}:::core
    DB[("🗄️ PostgreSQL<br/>Redis")]:::data
    RT["⚡ Soketi<br/>live events"]:::data
    AI["🧠 AI service<br/>face match · RAG · agents"]:::ai
  end
  VS ==>|signed licence| API
  SM --> API
  EM --> API
  PF -.->|sync when online| API
  API <--> DB
  API --> RT
  API <-->|on-prem inference| AI
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  classDef field fill:#059669,stroke:#6ee7b7,stroke-width:2px,color:#ffffff
  classDef ai fill:#9333ea,stroke:#d8b4fe,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef data fill:#1e1b4b,stroke:#818cf8,stroke-width:2px,color:#e0e7ff
  classDef cloud fill:#db2777,stroke:#f9a8d4,stroke-width:2px,color:#ffffff
  style vendor fill:#1a0b16,stroke:#db2777,stroke-width:2px,color:#f9a8d4
  style campus fill:#0d0d1a,stroke:#4f46e5,stroke-width:2px,color:#7dd3fc
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

## Two editions, one architecture

AEMMS ships as two editions for two national exam regulators: **KNEC** for teacher-training colleges and **CDACC** for TVET (technical and vocational) colleges. Each edition is its own release line with its own apps and server, so one regulator's rules never leak into the other, while both share the same architecture and licence authority.

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','primaryColor':'#16162a','primaryTextColor':'#f0f0ff','primaryBorderColor':'#4f46e5','lineColor':'#0ea5e9','clusterBkg':'#0d0d1a','clusterBorder':'#4f46e5','titleColor':'#0ea5e9','edgeLabelBackground':'#16162a'}}}%%
flowchart LR
  VS["🔑 Vendor control plane<br/>one licence authority"]:::cloud
  subgraph ttc["🎓 KNEC edition · teacher-training colleges"]
    direction TB
    SMK["🖥️ SystemMate KNEC"]:::staff
    EMK["🔒 ExamMate KNEC"]:::trainee
    PFK["📱 Practicum Field<br/>teaching practice"]:::field
    APK{{"⚙️ Institution API<br/>KNEC"}}:::core
    SMK --> APK
    EMK --> APK
    PFK -.-> APK
  end
  subgraph tvet["🛠️ CDACC edition · TVET colleges"]
    direction TB
    SMC["🖥️ SystemMate CDACC"]:::staff
    EMC["🔒 ExamMate CDACC"]:::trainee
    PFC["📱 Attachment Field<br/>industrial attachment"]:::field
    APC{{"⚙️ Institution API<br/>CDACC"}}:::core
    SMC --> APC
    EMC --> APC
    PFC -.-> APC
  end
  VS ==> APK
  VS ==> APC
  classDef staff fill:#4f46e5,stroke:#a5b4fc,stroke-width:2px,color:#ffffff
  classDef trainee fill:#0284c7,stroke:#7dd3fc,stroke-width:2px,color:#ffffff
  classDef field fill:#059669,stroke:#6ee7b7,stroke-width:2px,color:#ffffff
  classDef core fill:#ea580c,stroke:#fdba74,stroke-width:3px,color:#ffffff
  classDef cloud fill:#db2777,stroke:#f9a8d4,stroke-width:2px,color:#ffffff
  style ttc fill:#0d0d1a,stroke:#4f46e5,stroke-width:2px,color:#a5b4fc
  style tvet fill:#0d0d1a,stroke:#059669,stroke-width:2px,color:#6ee7b7
  linkStyle default stroke:#0ea5e9,stroke-width:2px
```

## Licensing that works offline

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','actorBkg':'#4f46e5','actorBorder':'#a5b4fc','actorTextColor':'#ffffff','actorLineColor':'#6b6b8a','signalColor':'#0ea5e9','signalTextColor':'#0ea5e9','labelBoxBkgColor':'#f97316','labelBoxBorderColor':'#fdba74','labelTextColor':'#ffffff','loopTextColor':'#f97316','noteBkgColor':'#16162a','noteTextColor':'#f0f0ff','noteBorderColor':'#f97316','activationBkgColor':'#0ea5e9','activationBorderColor':'#7dd3fc','sequenceNumberColor':'#ffffff'}}}%%
sequenceDiagram
  autonumber
  participant V as 🔑 Vendor control plane
  participant C as ⚙️ Campus API
  participant U as 🖥️ Desktop clients
  rect rgba(219, 39, 119, 0.16)
    Note over V: Issue
    V->>V: Sign modules, seats and expiry with the Ed25519 private key
    V-->>C: Deliver the signed .lic file
  end
  rect rgba(14, 165, 233, 0.16)
    Note over C: Verify offline
    C->>C: Verify the signature with the configured public key
    C->>C: Enable licensed modules until expiry
  end
  rect rgba(5, 150, 105, 0.16)
    Note over C,U: Run
    U->>C: Sign in and work on the campus LAN
    C-->>U: Features allowed by the licence
  end
```

## Field supervision without signal

```mermaid
%%{init: {'theme':'base','themeVariables':{'fontSize':'15px','actorBkg':'#4f46e5','actorBorder':'#a5b4fc','actorTextColor':'#ffffff','actorLineColor':'#6b6b8a','signalColor':'#0ea5e9','signalTextColor':'#0ea5e9','labelBoxBkgColor':'#f97316','labelBoxBorderColor':'#fdba74','labelTextColor':'#ffffff','loopTextColor':'#f97316','noteBkgColor':'#16162a','noteTextColor':'#f0f0ff','noteBorderColor':'#f97316','activationBkgColor':'#0ea5e9','activationBorderColor':'#7dd3fc','sequenceNumberColor':'#ffffff'}}}%%
sequenceDiagram
  autonumber
  participant S as 📱 Supervisor (field app)
  participant L as 💾 On-device store
  participant A as ⚙️ Institution API
  participant M as 🖥️ SystemMate boards
  rect rgba(5, 150, 105, 0.16)
    Note over S,L: At a school or workplace, no signal
    S->>L: Record visit, attendance and rubric scores
    L-->>S: Saved locally, queued for sync
  end
  rect rgba(79, 70, 229, 0.16)
    Note over S,A: Back online
    S->>A: Sync queued records
    A-->>S: Confirmed
    A->>M: Update supervision boards
  end
```

## Key engineering decisions

- **Campus-owned, commercially SaaS.** Each institution runs its own server on-site; Savvytex issues **Ed25519-signed licence files** that the campus server verifies offline. The business model is subscription, but exams never depend on an external connection.
- **One API, many clients.** Desktop, mobile and AI all speak to the same Django REST API, organised as controllers → repositories → models so each product line stays consistent.
- **Real-time without a SaaS dependency.** WebSocket events are served by a self-hosted Pusher-compatible server (Soketi), so live dashboards work on a closed LAN.
- **AI that never leaves the building.** Face verification (InsightFace on ONNX Runtime) and the learning assistant (Ollama models with retrieval over course material) run on campus hardware. No student data goes to a third-party AI provider.
- **Two regulator editions.** KNEC and CDACC editions share one architecture but ship as separate release lines, so regulator-specific rules stay isolated.
- **Deploy anywhere.** Docker Compose for development; Proxmox containers on campus in production.

## Stack

**Backend:** Python, Django REST Framework, SimpleJWT, PostgreSQL, Redis, Celery, Nginx, Gunicorn, Soketi
**Desktop:** Electron Forge, Vue 3, TypeScript, Pinia, TanStack Query, CKEditor 5, FullCalendar, Tailwind, AdminLTE
**Mobile:** Capacitor, Vue 3, Android
**AI:** FastAPI, InsightFace, ONNX Runtime, OpenCV, Ollama
**Infrastructure:** Docker, Proxmox, GitHub Actions

## Outcome

- Running in production on a college campus (see the [campus network case study](chesta-campus-network.md)).
- Full academic cycle live on campus hardware: authoring, sitting, marking, release and transcripts.
- More than 1,400 commits across the platform's repositories since February 2026.

---

**Need a platform like this built?** [Email me](mailto:eng.felixnyachio@gmail.com?subject=Platform%20project%20inquiry) · [WhatsApp](https://wa.me/254748650011)
