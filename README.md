# 🛡️ SubSentry

[![OpenGAP Spec 0.1.0](https://img.shields.io/badge/OpenGAP-0.1.0-blue.svg)](https://opengitagent.org)
[![GitAgent Passport](https://img.shields.io/badge/GitAgent%20Passport-Ready-brightgreen.svg)](https://app.hidevs.xyz/passport/submit)
[![Category](https://img.shields.io/badge/Category-Finance-emerald.svg)](https://app.hidevs.xyz/passport/submit)
[![Compliance](https://img.shields.io/badge/Compliance-GDPR%20%7C%20PCI--DSS-orange.svg)](EXPLAINABILITY.md)

**Stop silent money leaks. Own your subscriptions.**

SubSentry is an open-source **subscription management dashboard** that helps users track recurring payments, free trials, upcoming renewals, and automatically detect subscriptions using Gmail (read-only access).

This project is built as part of **Geekhaven OpenCode** and is designed to be beginner-friendly, production-oriented, and fully open-source.

---

## ✨ Features (OpenCode MVP)

- 🔐 Authentication using **Clerk**
- 📊 Subscription dashboard with monthly & yearly spend
- ✏️ Add, edit, and delete subscriptions
- ⏰ Track upcoming renewals and free trials
- 📥 Gmail email ingestion (read-only, keyword-based)
- 🗂️ Filter subscriptions by category
- 📈 Dashboard summary widgets

---

## 🧱 Tech Stack

### Frontend

- Next.js (App Router)
- Clerk Authentication
- Tailwind CSS

### Backend

- Node.js
- Express.js
- MongoDB
- Mongoose

### Deployment

- Frontend: Vercel
- Backend: Local (optional cloud deployment)

---

## 📁 Project Structure

```
subsentry/
├── client/                 # Next.js frontend
├── server/                 # Node + Express backend
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
├── README.md
└── CONTRIBUTING.md
```

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/<your-org>/subsentry.git
cd subsentry
```

---

### 2️⃣ Frontend Setup

```bash
cd client
npm install
npm run dev
```

Create `.env.local`:

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_key
CLERK_SECRET_KEY=your_secret
```

---

### 3️⃣ Backend Setup

```bash
cd server
npm install
npm run dev
```

Create `.env`:

```env
MONGO_URI=your_mongodb_uri
PORT=5000

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REDIRECT_URI=http://localhost:5000/api/auth/google/callback
```

---

## 🔐 Gmail Access & Privacy

SubSentry uses **Gmail read-only access** to detect subscription-related emails.

- ✔ Can read email content
- ❌ Cannot send emails
- ❌ Cannot delete or modify emails

**Scope used:**

```
https://www.googleapis.com/auth/gmail.readonly
```

We never store email content permanently and never access emails without user consent.

---

## 🤝 Contributing (OpenCode)

- Pick an issue and comment to get assigned
- Fork the repository
- Create a feature branch in your fork
- Open a Pull Request to `main`
- One PR per issue

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for detailed guidelines.

---

## 📜 License

MIT License

---

## 🧪 Scripts

### Frontend

```bash
npm run dev       # start dev server
npm run build     # build for production
npm run start     # start production server
```

### Backend

```bash
npm run dev       # start server with nodemon
npm start         # start server
```

---

## 🧰 Environment Variables

### Frontend (`client/.env.local`)

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
```

### Backend (`server/.env`)

```env
MONGO_URI=
PORT=5000
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REDIRECT_URI=http://localhost:5000/api/auth/google/callback
```

> ⚠️ Never commit `.env` files. Use `.env.example` if needed.

---

## 🔐 Security & Privacy Notes

- Gmail access is **read-only** and limited to subscription detection
- OAuth tokens are stored securely and scoped minimally
- No emails are modified, deleted, or sent
- No sensitive email content is persisted

---

## 🧩 FAQ

**Q: Why Gmail read-only?**
A: It ensures maximum safety and avoids Google verification requirements.

**Q: Can I add subscriptions manually?**
A: Yes, manual entry is fully supported.

**Q: Is this production-ready?**
A: It is an MVP built for OpenCode with production-grade patterns.

---

## 🌱 Roadmap (Post OpenCode)

- Advanced email parsing
- Dark-pattern detection
- Browser extension
- Team/workspace dashboards

---

## 🙌 Acknowledgements

Built with ❤️ during **Geekhaven OpenCode**.

---

## GitAgent Passport Qualification

This repository is fully compliant with the **OpenGAP Spec 0.1.0** standard and qualified for the **HiDevs GitAgent Passport**:

- **Checkpoint 1 (Validate):** Verified OpenGAP spec 0.1.0 compliance via [`agent.yaml`](agent.yaml), [`SOUL.md`](SOUL.md), [`skills/`](skills/), and [`tools/`](tools/).
- **Checkpoint 2 (Explain):** Comprehensive 5-section transparency report in [`EXPLAINABILITY.md`](EXPLAINABILITY.md) detailing read-only Gmail ingestion, recurring spend forecasting, PCI-DSS/GDPR compliance, and failure mode mitigations.
- **Checkpoint 3 (Export):** Cross-framework export compatibility tested across OpenAI SDK, CrewAI, Claude Code, and Lyzr.
- **Target Category:** **`Finance`** (Subscription Tracking & Recurring Expense Auditing).
