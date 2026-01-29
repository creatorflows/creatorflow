Perfect! I'll give you the **exact copy-paste content** for `DEVELOPMENT_GUIDE.md`. This is a complete, ready-to-use development guide for your team.

## 📄 **Content for `docs/DEVELOPMENT_GUIDE.md`**

Copy and paste **all of this** into your empty `DEVELOPMENT_GUIDE.md` file:

````markdown
# 🛠️ Development Guide

This guide explains how to set up, work on, and contribute to CreatorFlow.

## 🚀 Quick Start

### Prerequisites

Make sure you have installed:

- **Node.js 18+** ([Download](https://nodejs.org))
- **Git** ([Download](https://git-scm.com))
- **VS Code** (recommended) or any editor
- **PostgreSQL** (optional for now, we'll use SQLite for development)

### Step 1: Clone and Install

```bash
# Clone the repository
git clone https://github.com/creatorflows/creatorflow.git
cd creatorflow

# Install dependencies for the whole project
npm install

# Install frontend dependencies
cd apps/web
npm install

# Install backend dependencies
cd ../api
npm install
cd ../..  # Go back to project root
```
````

### Step 2: Environment Setup

```bash
# Create environment files
cp .env.example .env
cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env

# Edit the files with your local settings
# For quick start, you can leave defaults for now
```

### Step 3: Database Setup

```bash
# Navigate to backend
cd apps/api

# Generate Prisma client (first time)
npx prisma generate

# For development, we'll use SQLite (no installation needed)
# The .env file should have: DATABASE_URL="file:./dev.db"

# Create database and tables
npx prisma db push

# Optional: Seed with sample data
npx prisma db seed
```

### Step 4: Start Development Servers

```bash
# From project root, start both frontend and backend
npm run dev

# Or start them separately:
# Terminal 1 (frontend):
cd apps/web && npm run dev

# Terminal 2 (backend):
cd apps/api && npm run dev
```

✅ **You should now have:**

- Frontend running at: `http://localhost:3000`
- Backend running at: `http://localhost:3001`
- API health check: `http://localhost:3001/health` should return `{"status":"ok"}`

## 📁 Project Structure Overview

```
creatorflow/
├── apps/
│   ├── web/                 # React frontend (PORT 3000)
│   │   ├── src/
│   │   │   ├── components/  # Reusable UI components
│   │   │   ├── pages/       # Page components
│   │   │   ├── hooks/       # Custom React hooks
│   │   │   ├── lib/         # Utilities and config
│   │   │   ├── styles/      # CSS/Tailwind files
│   │   │   └── App.tsx      # Main app component
│   │   └── package.json
│   │
│   └── api/                 # Node.js backend (PORT 3001)
│       ├── src/
│       │   ├── routes/      # API route definitions
│       │   ├── controllers/ # Request handlers
│       │   ├── middleware/  # Auth, validation, etc.
│       │   ├── models/      # Database models (Prisma)
│       │   └── utils/       # Helper functions
│       ├── prisma/          # Database schema
│       └── package.json
├── docs/                    # Documentation
└── package.json            # Root package (monorepo)
```

## 💻 Development Workflow

### 1. Creating a New Feature

```bash
# 1. Update your local main branch
git checkout main
git pull origin main

# 2. Create a new branch for your feature
git checkout -b feature/your-feature-name
# Examples: feature/user-auth, feature/file-upload, feature/task-board

# 3. Make your changes and commit
git add .
git commit -m "feat: add user authentication system"

# 4. Push to GitHub
git push origin feature/your-feature-name

# 5. Create a Pull Request on GitHub
# Go to: https://github.com/creatorflows/creatorflow/pulls
```

### 2. Commit Message Convention

We use conventional commits:

- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `style:` - Code formatting
- `refactor:` - Code restructuring
- `test:` - Adding tests
- `chore:` - Maintenance tasks

### 3. Code Review Process

1. Create Pull Request (PR)
2. Request review from at least 1 team member
3. Address review comments
4. Once approved, squash and merge
5. Delete the feature branch

## 🧪 Testing

### Running Tests

```bash
# Run all tests
npm test

# Run frontend tests only
cd apps/web && npm test

# Run backend tests only
cd apps/api && npm test

# Run tests in watch mode
npm test -- --watch
```

### Test Structure

```
apps/web/src/__tests__/     # Frontend tests
apps/api/src/__tests__/     # Backend tests
```

## 🐛 Debugging

### Common Issues

**Issue:** "Port already in use"

```bash
# Find what's using the port
lsof -i :3000  # For macOS/Linux
# or
netstat -ano | findstr :3000  # For Windows

# Kill the process or change port in .env
```

**Issue:** "Cannot find module"

```bash
# Reinstall dependencies
rm -rf node_modules
npm install
```

**Issue:** "Prisma client not generated"

```bash
cd apps/api
npx prisma generate
```

### VS Code Debugging

Create `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Backend",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/apps/api/src/index.ts",
      "runtimeArgs": ["--nolazy", "-r", "ts-node/register"],
      "sourceMaps": true
    }
  ]
}
```

## 🔧 Useful Scripts

### From Project Root

```bash
npm run dev          # Start both frontend and backend
npm run build        # Build for production
npm test             # Run all tests
```

### Frontend (apps/web)

```bash
npm run dev          # Start dev server (port 3000)
npm run build        # Build for production
npm run preview      # Preview production build
npm run lint         # Check code quality
```

### Backend (apps/api)

```bash
npm run dev          # Start dev server (port 3001)
npm run build        # Build for production
npm start            # Start production server
npx prisma studio    # Open database GUI
npx prisma migrate dev  # Create migration
```

## 🗄️ Database Commands

```bash
# Generate Prisma client (after schema changes)
npx prisma generate

# Create and apply migration
npx prisma migrate dev --name "add_user_table"

# Reset database (development only)
npx prisma migrate reset

# Open Prisma Studio (database GUI)
npx prisma studio

# Check database status
npx prisma db pull
```

## 🌐 API Development

### Adding a New Endpoint

1. **Create route** in `apps/api/src/routes/`

```javascript
// Example: apps/api/src/routes/tasks.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.json({ message: "Get all tasks" });
});

router.post("/", (req, res) => {
  res.json({ message: "Create task" });
});

module.exports = router;
```

2. **Register route** in `apps/api/src/index.js`

```javascript
app.use("/api/tasks", require("./routes/tasks"));
```

3. **Test with curl or Postman**

```bash
curl http://localhost:3001/api/tasks
```

## 🎨 Frontend Development

### Creating a New Component

```typescript
// apps/web/src/components/Button.tsx
import React from 'react';

interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  variant?: 'primary' | 'secondary';
}

export const Button: React.FC<ButtonProps> = ({
  children,
  onClick,
  variant = 'primary'
}) => {
  return (
    <button
      className={`px-4 py-2 rounded ${
        variant === 'primary'
          ? 'bg-blue-600 text-white'
          : 'bg-gray-200 text-gray-800'
      }`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

### Adding a New Page

1. Create file in `apps/web/src/pages/`
2. Add route in `apps/web/src/App.tsx`
3. Import and use components

## 📦 Dependency Management

### Adding a New Package

```bash
# Frontend package
cd apps/web
npm install package-name

# Backend package
cd apps/api
npm install package-name

# Dev dependency (for all)
npm install -D package-name
```

### Updating Packages

```bash
# Check for updates
npm outdated

# Update all packages
npm update

# Update specific package
npm install package-name@latest
```

## 🚢 Deployment Checklist

Before deploying to production:

### Frontend

- [ ] Build passes: `npm run build`
- [ ] No TypeScript errors
- [ ] All environment variables set
- [ ] API URL configured correctly

### Backend

- [ ] Tests pass: `npm test`
- [ ] Database migrations applied
- [ ] Environment variables secure
- [ ] CORS configured for production domain

## 🆘 Getting Help

1. **Check this guide** first
2. **Look at existing code** for patterns
3. **Ask in team chat** (Slack/WhatsApp/Discord)
4. **Create GitHub Issue** for bugs
5. **Tag @team-lead** in PRs for urgent reviews

## ✅ Development Standards

### Code Style

```bash
- Use **TypeScript** for type safety
- Follow **ESLint** rules (configured in project)
- Use **Prettier** for formatting (auto-format on save)
- Write meaningful **comments** for complex logic
```

### Performance

```bash
- Minimize bundle size (check with `npm run build`)
- Use **React.memo** for expensive components
- Implement **pagination** for large data sets
- Optimize **database queries** (use Prisma includes wisely)
```

### Security

```bash
- Never commit `.env` files
- Validate all user input
- Use **parameterized queries** (Prisma handles this)
- Implement **rate limiting** on APIs
- Use **HTTPS** in production
```

---

**Need more help?** Check our [API Reference](api.md) or ask a team member!
