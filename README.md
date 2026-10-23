
# 🎓 Smart Attendance App

**Proxy-proof attendance using animated, multi-chunk QR codes and device binding.**

Built for Smart India Hackathon 2025.

[**Live Demo**](https://sih-2025-seven-kappa.vercel.app/)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [The Problem](#-the-problem)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [Anti-Proxy Security Layers](#-anti-proxy-security-layers)
- [Tech Stack](#%EF%B8%8F-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#%EF%B8%8F-getting-started)
- [Environment Variables](#-environment-variables)
- [Usage](#-usage)
- [Deployment](#-deployment)
- [Roadmap](#%EF%B8%8F-roadmap)
- [Contributing](#-contributing)
- [Team](#-team)
- [License](#-license)

---

## ✨ Overview

**Smart Attendance App** is a full-stack web application that makes attendance tracking fast, secure, and effortless for educational institutions.

Instead of a single static QR code that can be photographed and shared, the app displays an **animated sequence of QR codes**. A student must scan **every chunk** of the sequence to mark attendance, which only works if they are physically in front of the teacher's screen. Combined with **device binding** and **role-based authentication**, this makes proxy attendance very difficult.

Teachers and students each get their own dashboard, with live attendance updates, history, and analytics.

---

## 🎯 The Problem

Traditional attendance (roll calls, paper sheets, static QR codes) has three major issues:

| Problem | Impact |
|---|---|
| **Proxy attendance** | Students mark attendance for absent friends |
| **Static QR codes** | A screenshot of the QR can be shared and scanned from anywhere |
| **Manual effort** | Roll calls waste class time; paper records are hard to analyze |

This project solves them with a dynamic QR system, device binding, and automated record keeping.

---


## 🚀 Key Features

- ✅ **Proxy-Proof Attendance:** An animated, multi-chunk QR code must be scanned in full to mark attendance.
- ✅ **Role-Based Dashboards:** Separate dashboards for teachers and students.
  - **Teacher Dashboard**
    - Start and stop attendance sessions
    - View real-time attendance status
    - Manually mark or edit attendance
    - View and manage class schedules
  - **Student Dashboard**
    - Scan QR codes to mark attendance
    - View attendance history and statistics
    - Check upcoming classes
- ✅ **Real-Time Updates:** Attendance logs update live on the teacher's dashboard.
- ✅ **Secure Authentication:** JWT-based, role-aware login with separate Teacher and Student records.
- ✅ **Device Binding:** A student can only mark attendance from the device they registered with.
- ✅ **Data Visualization:** Charts and graphs for attendance analytics.
- ✅ **Responsive Design:** Works on desktops, tablets, and mobile devices.

---

## 🧠 How It Works

```
Teacher                         Server                          Student
   |                               |                               |
   |-- 1. Start session ---------->|                               |
   |                               |-- 2. Generate session ID      |
   |                               |      + sequence of chunks     |
   |<-- 3. Animated QR (chunks) ---|                               |
   |                               |                               |
   |   (QR frames change rapidly)  |                               |
   |                               |<-- 4. Scan all chunks --------|
   |                               |<-- 5. Submit full sequence ---|
   |                               |-- 6. Verify vs. stored seq.   |
   |<-- 7. Live attendance update -|-- 8. Mark attendance -------->|
```

1. **Teacher starts a session:** The teacher selects a class from their dashboard and starts an attendance session.
2. **Dynamic QR generation:** The server creates a unique session ID and a sequence of data chunks. The teacher's dashboard displays them as an animated QR code, where each frame is a different chunk.
3. **Student scans:** The student opens the scanner in their dashboard. It collects every unique chunk as it appears on the teacher's screen.
4. **Verification:** Once all chunks are collected, the student's device sends the full sequence to the server, which verifies it against the sequence stored for that session.
5. **Attendance marked:** If everything matches (and the device is the one registered to that student), attendance is recorded.
6. **Live feedback:** The teacher's dashboard shows which students have successfully marked attendance.

---

## 🔐 Anti-Proxy Security Layers

| Layer | What it prevents |
|---|---|
| **Animated multi-chunk QR** | A single screenshot or photo cannot contain the full sequence |
| **Sequence verification on the server** | Guessing or partially scanning the QR fails verification |
| **Device binding** | A student cannot mark attendance from someone else's device |
| **JWT role-based auth** | Students cannot access teacher-only actions |
| **Separate Teacher / Student collections** | Clear separation of data and permissions |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Next.js (React), TypeScript, Tailwind CSS, ShadCN UI |
| **Backend** | Node.js, Express.js |
| **Database** | MongoDB (Atlas) |
| **Authentication** | JWT (JSON Web Tokens) |
| **Animation** | Framer Motion |
| **Charts & Visualization** | Recharts, Chart.js |
| **Deployment** | Vercel |

---

## 📁 Project Structure

```
Smart-Attendance-App/
├── app/            # Next.js App Router pages and layouts
├── components/     # Reusable UI components
├── hooks/          # Custom React hooks
├── lib/            # Shared utilities and helpers
├── public/         # Static assets
├── styles/         # Global styles
├── components.json # ShadCN UI configuration
├── next.config.mjs # Next.js configuration
├── package.json
└── tsconfig.json
```

---

## ⚙️ Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- npm or yarn
- A [MongoDB Atlas](https://www.mongodb.com/atlas) account (or a local MongoDB instance)

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/himanshu-rai-19/Smart-Attendance-App.git
   cd Smart-Attendance-App
   ```

2. **Install dependencies**

   ```bash
   npm install
   # or
   yarn install
   ```

3. **Set up environment variables**

   Create a `.env` file in the project root (see [Environment Variables](#-environment-variables)).

4. **Run the development server**

   ```bash
   npm run dev
   ```

5. **Open the app** at [http://localhost:3000](http://localhost:3000).

---

## 🔑 Environment Variables

Create a `.env` file in the root of the project:

```env
MONGODB_URL=<your_mongodb_connection_string>
JWT_SECRET=<a_long_random_secret_string>
NODE_ENV=development
```

| Variable | Description |
|---|---|
| `MONGODB_URL` | MongoDB connection string (Atlas or local) |
| `JWT_SECRET` | Secret used to sign and verify JWTs. Use a long, random value |
| `NODE_ENV` | `development` or `production` |

> ⚠️ **Never commit your `.env` file.** Make sure it is listed in `.gitignore`.

---

## 📖 Usage

**As a teacher**
1. Register or log in with a teacher account.
2. Open your dashboard and select a class.
3. Click **Start Session**. The animated QR code appears.
4. Keep the QR visible on the projector or screen until students have scanned.
5. Click **Stop Session** to close attendance. You can edit entries manually if needed.

**As a student**
1. Register or log in with a student account. Your device is bound on registration.
2. Open the scanner from your dashboard.
3. Point your camera at the teacher's animated QR code and hold steady until all chunks are captured.
4. Your attendance is marked automatically. Check your history and stats anytime.

---

## 🌐 Deployment

The app is deployed on **Vercel**.

To deploy your own copy:

1. Push the repository to your GitHub account.
2. Import the project in [Vercel](https://vercel.com/new).
3. Add the environment variables (`MONGODB_URL`, `JWT_SECRET`, `NODE_ENV=production`) in the Vercel project settings.
4. Click **Deploy**.

---

## 🗺️ Roadmap

Ideas for future improvements (edit this list to match your plans):

- [ ] Optional geolocation / geofence check for extra verification
- [ ] Rate limiting on attendance submission endpoints
- [ ] Stronger device identification beyond a locally stored ID
- [ ] Export attendance reports (CSV / PDF)
- [ ] Email or push notifications for low attendance
- [ ] Admin dashboard for institution-level management
- [ ] Automated tests and CI pipeline

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request
---

## 👥 Team

Built for **Smart India Hackathon 2025**.

| Name | GitHub | LinkedIn |
|---|---|---|
| Himanshu Rai | [@himanshu-rai-19](https://github.com/himanshu-rai-19) | [Profile](https://www.linkedin.com/in/himanshu-rai-72233a342/) |
| Anurag Panwar |

---

## 📜 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

<div align="center">

If you found this project useful, consider giving it a ⭐

</div>
