<div align="center">

# ⚡ VirtuSync

### *Seamless Real-Time Video Conferencing & Direct Messaging Platform*

[![Live Demo](https://img.shields.io/badge/Live_Demo-virtusync.onrender.com-brightgreen?style=for-the-badge&logo=render)](https://virtusync.onrender.com/)
[![Node.js](https://img.shields.io/badge/Node.js-v18+-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![Socket.io](https://img.shields.io/badge/Socket.io-4.x-010101?style=for-the-badge&logo=socketdotio&logoColor=white)](https://socket.io/)
[![WebRTC](https://img.shields.io/badge/WebRTC-Peer_to_Peer-333333?style=for-the-badge&logo=webrtc&logoColor=white)](https://webrtc.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14+-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg?style=for-the-badge)](https://opensource.org/licenses/ISC)

<p align="center">
  <b>VirtuSync</b> is a modern, full-stack, real-time communication platform enabling high-definition peer-to-peer video calls, instant messaging, screen sharing, and user presence tracking. Built with WebRTC mesh architecture, Express, Socket.io, and PostgreSQL.
</p>

[🌐 Visit Live App](https://virtusync.onrender.com/) • [✨ Features](#-features) • [🏗️ Architecture](#%EF%B8%8F-system-architecture) • [🚀 Quick Start](#-getting-started) • [🔑 Environment Setup](#-environment-variables)

</div>

---

## 📖 Table of Contents

- [✨ Features](#-features)
- [🏗️ System Architecture](#%EF%B8%8F-system-architecture)
- [🛠️ Tech Stack](#%EF%B8%8F-tech-stack)
- [📁 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Database Setup](#database-setup)
  - [Installation \& Local Run](#installation--local-run)
- [🔑 Environment Variables](#-environment-variables)
- [📡 Real-Time Signals \& Socket Events](#-real-time-signals--socket-events)
- [☁️ Deployment Guide](#%EF%B8%8F-deployment-guide)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

## ✨ Features

### 📹 Peer-to-Peer Video Calls (WebRTC)
- **HD Video & Audio:** Ultra-low latency direct P2P audio/video streaming via WebRTC peer connections.
- **Media Controls:** Dynamic controls to mute/unmute microphone, enable/disable video camera, and toggle screen sharing.
- **Direct Room Code:** Create or join video conference rooms instantly via a unique Room ID or invitation link.
- **Temporary Guest Access:** Join calls instantly without forcing account registration or login.

### 💬 Real-Time Instant Messaging (Socket.io & PostgreSQL)
- **1-on-1 Direct Messaging:** Fast, reliable messaging powered by WebSockets.
- **Persistent Chat Logs:** All chat histories securely stored in PostgreSQL for continuous conversations.
- **Recent Contacts List:** Quick access sidebar showing recent conversations and contact list.

### 🔒 Authentication & User Presence
- **Secure Authentication:** Passport.js session-based authentication with bcrypt password hashing.
- **Live User Presence:** Real-time online/offline indicators updated dynamically across clients.
- **User Profiles:** Personalized user dashboard and profile management.

---

## 🏗️ System Architecture

```mermaid
flowchart TB
    subgraph Client["Client Browser"]
        UI["EJS Views / Tailwind CSS"]
        RTC["WebRTC Peer Connection"]
        SIO_C["Socket.io Client"]
    end

    subgraph Server["Node.js / Express Backend"]
        HTTP["Express HTTP Server"]
        AUTH["Passport.js / Session Auth"]
        SIO_S["Socket.io Signaling Server"]
    end

    subgraph Database["Database Layer"]
        PG[("PostgreSQL / Neon Database")]
    end

    UI <--> HTTP
    HTTP <--> AUTH
    AUTH <--> PG
    SIO_C <--"Signaling & Live Chat"--> SIO_S
    SIO_S <--"Store Messages"--> PG
    RTC <=="P2P Video / Audio Stream"==> RTC
```

---

## 🛠️ Tech Stack

| Category | Technology | Usage Description |
| :--- | :--- | :--- |
| **Frontend** | HTML5, EJS, Tailwind CSS, JavaScript (ES6+) | Dynamic server-rendered UI views and responsive media layouts. |
| **Backend** | Node.js, Express.js | Core Web application server, HTTP routing, and API handling. |
| **Real-time Comms** | Socket.io, WebRTC | WebSocket signaling for P2P connection handshake and live chat messaging. |
| **Database** | PostgreSQL, `pg` (Node-Postgres) | Persistent storage for users, authentication credentials, and chat messages. |
| **Security & Auth** | Passport.js (Local Strategy), `bcrypt`, `express-session` | Session cookies, secure password hashing, and route protection. |
| **Utilities** | `uuid`, `dotenv` | Unique room/guest ID generation and environment variable configuration. |

---

## 📁 Project Structure

```text
virtusync/
├── public/
│   ├── css/
│   └── style.css            # Global CSS styles & layout rules
├── views/
│   ├── partials/            # Header, footer, and navigation partials
│   ├── calling.ejs          # Active video call interface & media controls
│   ├── directcall.ejs       # Instant room code joining & guest landing page
│   ├── index.ejs            # Main landing page
│   ├── login.ejs            # User login view
│   ├── profile.ejs          # User chat & dashboard interface
│   ├── register.ejs         # User registration view
│   └── start.ejs            # Call initialization selector
├── .env.example             # Template for environment variables
├── .gitignore               # Ignored files (node_modules, .env, logs)
├── index.js                 # Main server initialization, database connection & Socket.io logic
├── package.json             # NPM dependencies and scripts
└── README.md                # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed on your machine:
- **Node.js** (v18.x or higher)
- **npm** (v9.x or higher)
- **PostgreSQL** database instance (Local or Cloud provider like Neon / Railway)

---

### Database Setup

Execute the following SQL queries in your PostgreSQL database to create the required tables:

```sql
-- Create Users Table
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create Messages Table
CREATE TABLE IF NOT EXISTS messages (
    id SERIAL PRIMARY KEY,
    sender_id INT NOT NULL,
    recipient_id INT NOT NULL,
    message_text TEXT NOT NULL,
    timestamp TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

---

### Installation & Local Run

1. **Clone the Repository**
   ```bash
   git clone https://github.com/bidhuripriyanshu/virtusync-Video-Conferencing-Platform-.git
   cd virtusync-Video-Conferencing-Platform-
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Configure Environment Variables**
   Create a `.env` file in the root directory:
   ```bash
   cp .env.example .env
   ```
   Fill in your PostgreSQL credentials and session secret (see [Environment Variables](#-environment-variables)).

4. **Start the Application**
   - **Development Mode (Auto-reload):**
     ```bash
     npx nodemon index.js
     ```
   - **Production Mode:**
     ```bash
     node index.js
     ```

5. **Access the App**
   Open your browser and navigate to:
   ```text
   http://localhost:3000
   ```

---

## 🔑 Environment Variables

Create a `.env` file in the root directory with the following configurations:

| Variable Name | Required | Default Value | Description |
| :--- | :--- | :--- | :--- |
| `PORT` | Optional | `3000` | Port number for the HTTP server |
| `SESSION_SECRET` | **Yes** | — | Secret key used to encrypt express session cookies |
| `DB_URL` | Optional* | — | Full PostgreSQL connection URI (e.g., `postgres://user:pass@host:5432/dbname`) |
| `PG_USER` | Optional* | `postgres` | PostgreSQL database user name |
| `PG_PASSWORD` | Optional* | — | PostgreSQL user password |
| `PG_HOST` | Optional* | `localhost` | Database host address |
| `PG_PORT` | Optional* | `5432` | Database port |
| `PG_DATABASE` | Optional* | `virtusync` | Name of the PostgreSQL database |

*\*Note: Provide either `DB_URL` OR the set of individual `PG_*` parameters.*

---

## 📡 Real-Time Signals & Socket Events

VirtuSync utilizes Socket.io events for managing signaling and messaging flows:

| Event Name | Direction | Description |
| :--- | :--- | :--- |
| `active-users` | Server ➔ Client | Broadcasts online user IDs to all connected clients |
| `private-message` | Client ➔ Server | Sends direct chat payload (recipient ID & message body) |
| `chat-message` | Server ➔ Client | Delivers real-time message payload to recipient socket |
| `join-room` | Client ➔ Server | Joins client to a WebRTC video room by Room ID |
| `user-connected` | Server ➔ Client | Notifies existing room participants of a new peer joining |
| `offer` / `answer` | Client ⇄ Client | WebRTC SDP handshake exchanged via signaling server |
| `ice-candidate` | Client ⇄ Client | Transmits ICE candidate data for network connectivity |

---

## ☁️ Deployment Guide

### Deploying to Render / Vercel

1. **Database:** Create a managed PostgreSQL database on [Neon.tech](https://neon.tech) or Render.
2. **Environment Setup:** Set environment variables in your deployment platform:
   - `SESSION_SECRET`
   - `DB_URL` (Use connection string with `?sslmode=require`)
3. **Build & Start Command:**
   - **Build Command:** `npm install`
   - **Start Command:** `node index.js`

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the Repository.
2. Create a Feature Branch (`git checkout -b feature/AmazingFeature`).
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the Branch (`git checkout -b feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the **ISC License**. See the `LICENSE` file for details.

---

<div align="center">
  <sub>Built with ❤️ by Siddhu & contributors</sub>
</div>


