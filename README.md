# CreatorFlow 🎬

**From raw to ready** — A collaborative workspace for social media production teams to manage tasks, share files, and coordinate content creation.

## 🚀 Live Demo & Status

- **Repository:** https://github.com/creatorflows/creatorflow
- **Status:** 🚧 MVP Under Active Development
- **Target Launch:** Q3 2024
- **Demo:** Coming soon at https://creatorflow.vercel.app

## ✨ Core Features

| Feature                | Status     | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| **Team Workspaces**    | ✅ Planned | Dedicated spaces per influencer/brand   |
| **Project Management** | ✅ Planned | Organize content by campaigns           |
| **Task Assignment**    | ✅ Planned | Assign to roles: Filmer, Editor, Writer |
| **File Versioning**    | ✅ Planned | Track revisions with comments           |
| **Timestamp Feedback** | ⏳ Backlog | Comment on specific video moments       |
| **Approval Workflow**  | ✅ Planned | Todo → In Progress → Review → Approved  |

## 🛠 Tech Stack

**Frontend**

- React 18 + TypeScript
- Vite (Build tool)
- Tailwind CSS (Styling)
- React Query (Data fetching)

**Backend**

- Node.js + Express
- TypeScript
- Prisma ORM (Database client)
- JWT Authentication

**Infrastructure**

- PostgreSQL (Database)
- Cloudflare R2 (File storage)
- Vercel (Frontend hosting)
- Railway (Backend hosting)

## 🏗 Project Architecture

creatorflow/
├── apps/
│ ├── web/ # Frontend React app
│ └── api/ # Backend REST API
├── docs/ # Project documentation
├── .github/ CI/CD & workflows
└── package.json # Monorepo configuration

## 📋 Getting Started (For Developers)

### Prerequisites

- Node.js 18+ and npm
- PostgreSQL 14+
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/creatorflows/creatorflow.git
cd creatorflow

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your database credentials

# Run database migrations
npx prisma migrate dev

# Start development servers
npm run dev
# Frontend: http://localhost:3000
# Backend: http://localhost:3001
```
