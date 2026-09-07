# PulseLive ⚡

PulseLive is a real-time polling and Q&A platform designed for presenters, teachers, and event hosts who need instant audience interaction. Built with React and Supabase, it provides live updates, interactive presentation views, and effortless audience onboarding via QR codes.

---

## 🌟 Key Features

- **Multiple Poll Formats:** Choose between classic multiple-choice voting or interactive Q&A sessions.
- **Instant Real-time Sync:** Powered by Supabase Realtime—votes and question submissions appear live without manual refreshes.
- **Presenter View:** Full-screen presentation view complete with custom-generated QR codes for quick audience access.
- **Poll Lifecycle Control:** Flexibly switch poll status across `Draft`, `Live`, and `Closed` states.

---

## 🛠️ Tech Stack

- **Frontend:** React 19, TypeScript, Vite
- **Routing:** React Router DOM
- **Backend & Database:** Supabase (PostgreSQL + Realtime Engine)
- **Utilities:** `qrcode.react` (QR generation)

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+ recommended)
- `pnpm` (recommended, but `npm` or `yarn` work fine)

### Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/your-username/pulselive.git](https://github.com/your-username/pulselive.git)
   cd pulselive
