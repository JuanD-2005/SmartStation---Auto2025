# Estacion Inteligente | Smart Home Security Monitoring

![Status](https://img.shields.io/badge/Status-Portfolio%20Project-success?style=for-the-badge)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=for-the-badge&logo=javascript&logoColor=000)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-CDN-38B2AC?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-Auth%20%7C%20Realtime%20DB%20%7C%20Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=000)
![IoT](https://img.shields.io/badge/IoT-ESP8266%20%2B%20Sensors-0A66C2?style=for-the-badge)

End-to-end IoT + Web Security Solution that combines physical sensors, real-time cloud sync, authentication, and event-driven alerts.

Designed as an academic project with production-like architecture decisions and portfolio-ready engineering practices.

## Why this project matters

Most security demos stop at sensor reading. This system goes further:

- It translates raw sensor signals into actionable threat diagnosis.
- It connects hardware events to a cloud-backed web application in real time.
- It adds identity, history, and notification layers to close the monitoring loop.

This demonstrates practical skills in full-stack integration, not just frontend screens.

## Recruiter Snapshot

- Domain: IoT, Smart Home, Real-Time Monitoring
- Scope: hardware integration + full web panel + authentication + cloud persistence + email alerting
- Core value: anomaly detection and alert traceability for home safety scenarios
- Delivery style: fast MVP with scalable Firebase foundation

## Live Feature Highlights

- Real-time dashboard showing:
  - Door/visitor status
  - Motion detection status
  - Ambient light status
- Rule-based anomaly engine for multi-sensor correlation.
- Alert history timeline persisted in Firebase Realtime Database.
- User authentication with Firebase Auth:
  - Email and password
  - Google sign-in
- Notification workflow via EmailJS when risky conditions appear.
- Arm/Disarm-style analysis toggle from the dashboard UI.

## Technical Impact

This portfolio project showcases:

- Event-driven thinking: reacts to sensor changes using real-time listeners.
- Data modeling: separates live telemetry and historical incidents.
- Security awareness: authenticated access and clear hardening roadmap.
- Product mindset: user flow includes register, login, monitoring, alerts, logout.
- Frontend UX: modern visual dashboard using Tailwind utility styling.

## System Architecture

```text
Sensors (PIR, LDR, Ultrasonic) + ESP8266
                |
                v
Firebase Realtime Database (sensor/*)
                |
                v
Web Dashboard (index.html)
  - subscribes to live data
  - computes anomaly diagnosis
  - stores alert records in /alertas
  - triggers email notifications (EmailJS)
                |
                v
Firebase Authentication
  - protected panel access
  - email/password and Google login
```

## Anomaly Detection Rules

The app evaluates these sensor combinations in real time:

- Motion detected + Visitor present + Dark -> Night intrusion
- Motion detected + Visitor present + Light -> Unauthorized daytime access
- No motion + Visitor present -> Suspicious door-area presence
- Motion detected + No visitor -> Potential internal movement with closed access
- No motion + Light -> Unnecessary lights on
- Motion detected + Out of range -> Ultrasonic sensor blocked

Fallback diagnosis:

- No matching pattern -> No anomalies detected

## Tech Stack

- Frontend: HTML5, JavaScript (ES modules), Tailwind CSS via CDN
- Backend platform: Firebase
  - Authentication
  - Realtime Database
  - Hosting
  - Cloud Functions scaffold in functions/
- Notifications: EmailJS
- Hardware: ESP8266 NodeMCU + PIR + LDR + HC-SR04

## Repository Structure

```text
.
|-- firebase.json
|-- index.html
|-- login.html
|-- register.html
|-- style.css
|-- gmail.png
`-- functions/
    |-- index.js
    `-- package.json
```

## Quick Start

### Prerequisites

- Node.js LTS
- npm
- Firebase CLI
- Firebase project with Auth and Realtime Database enabled

### Run locally

```bash
git clone <YOUR_REPOSITORY_URL>
cd "Estacion Inteligente"
npm install -g firebase-tools

cd functions
npm install
cd ..

firebase login
firebase use --add
firebase serve
```

Optional emulators:

```bash
firebase emulators:start
```

Default local URL: http://localhost:5000

## Deployment

Deploy all:

```bash
firebase deploy
```

Deploy only hosting:

```bash
firebase deploy --only hosting
```

Deploy only functions:

```bash
cd functions
npm run deploy
```

## Data Model

```json
{
  "sensor": {
    "movimiento": "Movimiento detectado | Sin movimiento",
    "visitante": "Visitante presente | Sin visitante | Fuera de rango",
    "estadoLuz": "Luz | Oscuro"
  },
  "alertas": {
    "-Nx...": {
      "usuario": "usuario@correo.com",
      "motivo": "Night intrusion",
      "fecha": "27/3/2026, 10:42:18"
    }
  }
}
```

## Security Notes

For public/open-source publication, apply these improvements:

- Move sensitive provider keys and service identifiers out of public frontend code.
- Use stricter Firebase Database Rules with auth-based write restrictions.
- Add server-side validation for alert creation.
- Rotate exposed tokens if repository history becomes public.
- Introduce role-based access for admin/operator views.

## Portfolio Positioning

If you are reviewing this as a recruiter or technical lead, this project demonstrates candidate readiness in:

- Full-stack integration (IoT + cloud + frontend)
- Real-time systems and reactive interfaces
- Product-oriented feature design under academic constraints
- Cloud-native workflow with Firebase ecosystem
- Fast prototyping with extensible architecture

## Suggested Next Iteration

- Move anomaly logic to Cloud Functions for centralized rules.
- Add severity scoring and incident prioritization.
- Add push notifications (FCM) alongside email.
- Add charts and filtering in alert analytics.
- Add unit/integration tests for rules engine reliability.

## Team

- Juan Paredes
- Jose Bravo
- Angel Vivas

Universidad Nacional Experimental del Tachira (UNET)  
Automatizacion de Procesos 2025 - Apertura del Salon Japones

---

### Contact

If you want to discuss this project, architecture choices, or collaboration opportunities, open an issue or reach out through your professional profile links.

