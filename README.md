<!-- Header -->
<p align="center">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=220&section=header&text=Eng.%20Felix%20Nyachio&fontSize=52&fontColor=ffffff&fontAlignY=38&desc=Full-Stack%20Engineer%20%E2%80%A2%20EdTech%20Architect%20%E2%80%A2%20Educator&descAlignY=58&descSize=18&animation=fadeIn" alt="Eng. Felix Nyachio"/>
</p>

<p align="center">
  <a href="https://github.com/flowser">
    <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=3200&pause=900&color=38BDF8&center=true&vCenter=true&width=720&lines=I+build+software+that+runs+colleges.;Django+%C2%B7+Vue+%C2%B7+Electron+%C2%B7+FastAPI+%C2%B7+Docker;Self-hosted+AI+on+school+networks.;Exam-mode+Wi-Fi+enforcement+with+MikroTik.;Risk-first+algorithmic+trading+on+MT5." alt="Typing intro"/>
  </a>
</p>

<p align="center">
  <a href="https://savvytexmarines.co.ke"><img src="https://img.shields.io/badge/Savvytex_Marines-Website-0ea5e9?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Website"/></a>
  <a href="mailto:engfelixnyachio@gmail.com"><img src="https://img.shields.io/badge/Email-Hire_me-ef4444?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
  <img src="https://img.shields.io/badge/Nairobi-Kenya-16a34a?style=for-the-badge&logo=googlemaps&logoColor=white" alt="Location"/>
  <img src="https://img.shields.io/badge/Open_to-Collaboration-a855f7?style=for-the-badge&logo=handshake&logoColor=white" alt="Open to collaboration"/>
</p>

<p align="center">
  <img src="https://komarev.com/ghpvc/?username=flowser&style=flat-square&color=0ea5e9&label=Profile+views" alt="Profile views"/>
</p>

---

## About me

I'm an engineer and technical educator in Nairobi. I lead engineering at **Savvytex Marines Ltd**, and I teach Electrical & Telecommunications Engineering at a Kenyan technical training institute. The people I teach are the same people who use my software, which keeps me honest.

Most of my work lives in **private production repositories**. It runs real campuses: exams, marking, lessons, industrial attachment, campus Wi-Fi, and elections, for colleges regulated by both **KNEC** and **CDACC**.

<table>
  <tr>
    <td align="center" width="25%"><h2>2,000+</h2><sub>commits across private production repos</sub></td>
    <td align="center" width="25%"><h2>20+</h2><sub>repositories shipped and maintained</sub></td>
    <td align="center" width="25%"><h2>4</h2><sub>platforms: desktop, web, mobile, on-prem AI</sub></td>
    <td align="center" width="25%"><h2>2</h2><sub>exam regulators served (KNEC and CDACC)</sub></td>
  </tr>
</table>

---

## Flagship: AEMMS, an education management platform

**AEMMS** is a full ecosystem I designed and built for teacher-training and TVET colleges. Staff author, mark, and release exams on a desktop app; trainees sit exams on a locked-down desktop client; supervisors assess teaching practice and industrial attachment from a mobile app that works offline; and an on-campus AI service handles face verification and learning assistance. It's all tied together by a Django API and licensed from a central vendor server.

```mermaid
flowchart TB
  subgraph campus["Campus LAN"]
    SM["SystemMate<br/>staff desktop"]
    EM["ExamMate<br/>trainee desktop"]
    PF["Practicum Field<br/>offline-first mobile"]
    AI["AI Service<br/>face ID, RAG, agents"]
    API[("School API<br/>Django, Postgres, Redis, Celery")]
    NM["NetworkMate<br/>exam-mode Wi-Fi"]
  end
  subgraph cloud["Savvytex cloud"]
    STX["Vendor server<br/>licensing, CMS, sync"]
  end
  SM --> API
  EM --> API
  PF --> API
  AI --> API
  API -.->|exam mode on| NM
  STX -->|signed licences| API
```

| Component | What it does | Built with |
|---|---|---|
| **SystemMate** | Staff desktop for lessons, LMS, assessment authoring (with math editing), marking, results release, timetables, and attachment/PoE boards | Electron Forge, Vue 3, TypeScript, Pinia, TanStack Query, CKEditor, FullCalendar |
| **ExamMate** | Trainee desktop for secure exam sitting, lessons, results, and attachment status | Electron, Vue 3, TypeScript, real-time via Soketi |
| **School API** | Multi-tenant institution backend with JWT auth, background jobs, device workers, and real-time events | Django REST Framework, PostgreSQL, Redis, Celery, Nginx, Docker |
| **Practicum Field** | Offline-first mobile app for teaching-practice and industrial-attachment supervision | Capacitor, Vue 3, Android |
| **AI Service** | On-premises AI that never leaves the school network: face verification, retrieval-augmented learning assistant, multi-mode agents | FastAPI, InsightFace, ONNX Runtime, OpenCV, Ollama |
| **Vendor and licensing** | Issues cryptographically signed licence files to each campus deployment | Django, Ed25519 |

Each product ships in two regulator-specific lines (KNEC for teacher-training colleges, CDACC for TVET institutes), deployed on Docker locally and Proxmox on campus.

---

## Other things I've built

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>NetworkMate</h3>
      A control room for school Wi-Fi. Tracks students and their devices, flags unauthorised hardware, and switches the whole campus into <b>exam mode</b> to block cheating. Integrates with MikroTik RouterOS and FreeRADIUS.<br/><br/>
      <img src="https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white"/>
      <img src="https://img.shields.io/badge/Vue-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white"/>
      <img src="https://img.shields.io/badge/MikroTik-293239?style=flat-square&logo=mikrotik&logoColor=white"/>
      <img src="https://img.shields.io/badge/CI-GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white"/>
    </td>
    <td width="50%" valign="top">
      <h3>TradeMate</h3>
      An MT5 scalping Expert Advisor plus a Python desktop monitor for 8 assets. Built <b>risk-first</b>: percentage risk per trade, daily loss caps, spread gates, news pauses, and strictly no martingale. Every change goes through backtest holdouts before it goes live.<br/><br/>
      <img src="https://img.shields.io/badge/MQL5-1E90FF?style=flat-square&logo=metatrader&logoColor=white"/>
      <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"/>
      <img src="https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white"/>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>Savvytex Platform</h3>
      The company's own stack: a CMS-driven public website, a staff operations console (social publishing, learning content, portfolios, licensing), and the API behind it.<br/><br/>
      <img src="https://img.shields.io/badge/Vue_3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white"/>
      <img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white"/>
      <img src="https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white"/>
      <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white"/>
    </td>
    <td width="50%" valign="top">
      <h3>VoteMate</h3>
      Campus election management: voter registers, ballots, live results over WebSockets. Runs as a self-contained Docker stack on a campus server.<br/><br/>
      <img src="https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white"/>
      <img src="https://img.shields.io/badge/Vue-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white"/>
      <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white"/>
      <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white"/>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>Nyumbané</h3>
      A household budgeting app for Kenyan families, shipped as four standalone clients (web, desktop, iOS/Android) on one Django API.<br/><br/>
      <img src="https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white"/>
      <img src="https://img.shields.io/badge/Capacitor-119EFF?style=flat-square&logo=capacitor&logoColor=white"/>
      <img src="https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white"/>
    </td>
    <td width="50%" valign="top">
      <h3>Hardware and infrastructure</h3>
      A home-lab Proxmox server hosting campus services; a USB smart-display host monitor for macOS and Proxmox; IoT water-flow monitoring; and <a href="https://github.com/flowser/bbGesture">bbGesture</a>, a gesture sensor that works through objects.<br/><br/>
      <img src="https://img.shields.io/badge/Proxmox-E57000?style=flat-square&logo=proxmox&logoColor=white"/>
      <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black"/>
      <img src="https://img.shields.io/badge/Arduino-00979D?style=flat-square&logo=arduino&logoColor=white"/>
    </td>
  </tr>
</table>

I also build research tooling: Bayesian density-estimation simulations (PyMC), survey-data pipelines for postgraduate research, and a generator that turns CDACC curricula into learning plans and session plans for my engineering classes.

---

## Tech stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=python,django,fastapi,ts,js,vue,vite,tailwind,bootstrap,electron&perline=10" alt="Languages and frameworks"/>
  <br/>
  <img src="https://skillicons.dev/icons?i=postgres,mysql,redis,docker,nginx,linux,bash,githubactions,git,php&perline=10" alt="Data and infrastructure"/>
  <br/>
  <img src="https://skillicons.dev/icons?i=laravel,androidstudio,arduino,opencv,vscode,apple,windows&perline=10" alt="Other tools"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Proxmox-E57000?style=flat-square&logo=proxmox&logoColor=white"/>
  <img src="https://img.shields.io/badge/MikroTik_RouterOS-293239?style=flat-square&logo=mikrotik&logoColor=white"/>
  <img src="https://img.shields.io/badge/Celery-37814A?style=flat-square&logo=celery&logoColor=white"/>
  <img src="https://img.shields.io/badge/Capacitor-119EFF?style=flat-square&logo=capacitor&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pinia-FFD859?style=flat-square&logo=pinia&logoColor=black"/>
  <img src="https://img.shields.io/badge/Ollama-000000?style=flat-square&logo=ollama&logoColor=white"/>
  <img src="https://img.shields.io/badge/ONNX_Runtime-005CED?style=flat-square&logo=onnx&logoColor=white"/>
  <img src="https://img.shields.io/badge/MQL5-1E90FF?style=flat-square&logo=metatrader&logoColor=white"/>
</p>

---

## How I work

- **Ship in small branches.** Feature branch, CI green, merge. Every repo has the same ritual.
- **Docs are part of the product.** Architecture diagrams, runbooks, and operator guides live next to the code.
- **Own the whole stack.** From the MikroTik router and the Proxmox host up to the Vue component the trainee clicks.
- **Keep data on-site.** School AI runs on the campus network, not in someone else's cloud.

---

## GitHub activity

<p align="center">
  <img width="100%" src="https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=flowser&theme=tokyonight" alt="Profile details"/>
</p>

<p align="center">
  <img src="https://streak-stats.demolab.com/?user=flowser&theme=tokyonight&hide_border=true&background=0d1117" alt="Contribution streak"/>
</p>

<p align="center">
  <img width="100%" src="https://ghchart.rshah.org/38bdf8/flowser" alt="Contribution chart"/>
</p>

---

## Let's build something

I'm open to EdTech partnerships, school and campus network projects, on-premises AI deployments, and freelance full-stack work.

<p align="center">
  <a href="mailto:engfelixnyachio@gmail.com"><img src="https://img.shields.io/badge/engfelixnyachio@gmail.com-ef4444?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"/></a>
  <a href="https://savvytexmarines.co.ke"><img src="https://img.shields.io/badge/savvytexmarines.co.ke-0ea5e9?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Website"/></a>
</p>

<p align="center">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:2c5364,50:203a43,100:0f2027&height=120&section=footer" alt="Footer"/>
</p>
