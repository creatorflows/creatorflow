# 📄 **Content for `docs/architecture.md`**

Copy and paste **all of this** into your empty `architecture.md` file:

```markdown
# 🏗️ System Architecture

## 📐 Overview

CreatorFlow follows a modern, decoupled architecture with clear separation between frontend and backend services. This document explains the technical design, component interactions, and infrastructure decisions.

## 🎯 Architecture Principles

1. **Separation of Concerns**: Frontend ≠ Backend ≠ Database
2. **API-First Design**: Backend serves REST API, frontend consumes it
3. **Stateless Backend**: No server-side sessions, JWT for auth
4. **Scalability**: Each component can scale independently
5. **Developer Experience**: Fast local setup, clear boundaries

## 🏢 High-Level Architecture Diagram
```

┌─────────────────────────────────────────────────────────────┐
│ Client Devices │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ Desktop │ │ Tablet │ │ Mobile │ │
│ │ Browser │ │ Browser │ │ Browser │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
│ HTTPS
┌─────────────────────────────────────────────────────────────┐
│ Load Balancer / CDN │
│ (Cloudflare / Vercel) │
└─────────────────────────────────────────────────────────────┘
│
┌─────────────────────────────────────────────────────────────┐
│ Frontend Application │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Single Page Application │ │
│ │ • React 18 + TypeScript │ │
│ │ • Vite Build System │ │
│ │ • Tailwind CSS │ │
│ │ • React Router v6 │ │
│ │ • React Query (Data Fetching) │ │
│ └──────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
│ REST API (HTTPS)
┌─────────────────────────────────────────────────────────────┐
│ Backend API Service │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Node.js + Express │ │
│ │ • TypeScript Runtime │ │
│ │ • JWT Authentication │ │
│ │ • Role-Based Access Control │ │
│ │ • Request Validation (Zod) │ │
│ │ • File Upload Handling │ │
│ │ • Rate Limiting │ │
│ └──────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
│ │ │
┌─────────┴─────────┐ ┌───────┴───────┐ ┌───────┴───────┐
│ PostgreSQL │ │ Redis │ │ Object │
│ Database │ │ Cache │ │ Storage │
│ • Primary Data │ │ • Sessions │ │ • Files │
│ • ACID Compliant │ │ • Rate Limiter│ │ • Assets │
└───────────────────┘ └───────────────┘ └───────────────┘

```

## 🖥️ Frontend Architecture

### Technology Stack
| Component | Technology | Purpose |
|-----------|------------|---------|
| **Framework** | React 18 | UI component library |
| **Language** | TypeScript | Type safety, better DX |
| **Build Tool** | Vite | Fast builds, HMR |
| **Styling** | Tailwind CSS | Utility-first CSS |
| **State Management** | React Context + Zustand | Global state |
| **Data Fetching** | React Query | Server state, caching |
| **Routing** | React Router v6 | Client-side navigation |
| **HTTP Client** | Axios | API requests |
| **Form Handling** | React Hook Form | Form validation |
| **UI Components** | shadcn/ui | Accessible components |

### Folder Structure
```

apps/web/src/
├── components/ # Reusable UI components
│ ├── ui/ # Basic components (Button, Input, Card)
│ ├── layout/ # Layout components (Header, Sidebar)
│ ├── features/ # Feature-specific components
│ └── shared/ # Shared across features
├── pages/ # Page components
│ ├── Home/
│ ├── Dashboard/
│ ├── Workspace/
│ └── Auth/
├── hooks/ # Custom React hooks
│ ├── useAuth.ts
│ ├── useWorkspace.ts
│ └── useToast.ts
├── lib/ # Third-party integrations
│ ├── axios.ts # HTTP client config
│ ├── supabase.ts # Database client
│ └── constants.ts # App constants
├── stores/ # State management
│ ├── auth.store.ts
│ └── ui.store.ts
├── types/ # TypeScript definitions
│ ├── api.types.ts
│ ├── user.types.ts
│ └── workspace.types.ts
├── utils/ # Helper functions
│ ├── format.ts # Date/string formatting
│ ├── validation.ts # Form validation
│ └── error.ts # Error handling
└── App.tsx # Root component

```

### Key Frontend Patterns
1. **Component Composition**: Small, focused components
2. **Custom Hooks**: Encapsulate reusable logic
3. **Error Boundaries**: Graceful error handling
4. **Suspense**: Loading states for async data
5. **Code Splitting**: Lazy-loaded routes

## ⚙️ Backend Architecture

### Technology Stack
| Component | Technology | Purpose |
|-----------|------------|---------|
| **Runtime** | Node.js 18+ | JavaScript runtime |
| **Framework** | Express.js | Web application framework |
| **Language** | TypeScript | Type safety |
| **ORM** | Prisma | Database client, migrations |
| **Validation** | Zod | Runtime type validation |
| **Authentication** | JWT + bcrypt | User auth, password hashing |
| **File Upload** | Multer + S3 SDK | File handling |
| **Caching** | Redis (optional) | Session cache, rate limiting |
| **Logging** | Winston/Pino | Structured logging |
| **Testing** | Jest + Supertest | Unit/integration tests |

### Folder Structure
```

apps/api/src/
├── config/ # Configuration files
│ ├── database.ts # DB connection
│ ├── storage.ts # S3/R2 config
│ └── constants.ts # App constants
├── middleware/ # Express middleware
│ ├── auth.middleware.ts
│ ├── validation.middleware.ts
│ ├── error.middleware.ts
│ └── rate-limit.middleware.ts
├── routes/ # API route definitions
│ ├── auth.routes.ts
│ ├── workspace.routes.ts
│ ├── project.routes.ts
│ ├── task.routes.ts
│ └── file.routes.ts
├── controllers/ # Request handlers
│ ├── auth.controller.ts
│ ├── workspace.controller.ts
│ └── file.controller.ts
├── services/ # Business logic
│ ├── auth.service.ts
│ ├── workspace.service.ts
│ └── file.service.ts
├── models/ # Data models (Prisma)
│ └── prisma/ # Prisma schema and client
├── utils/ # Helper functions
│ ├── apiResponse.ts # Standardized responses
│ ├── logger.ts # Logging utilities
│ └── validators.ts # Validation schemas
├── types/ # TypeScript definitions
│ ├── express/ # Extended Express types
│ └── api/ # API request/response types
└── app.ts # Express app setup

````

### API Design Principles
1. **RESTful Design**: Resource-based endpoints
2. **Versioning**: `/api/v1/` prefix for future compatibility
3. **Standard Responses**: Consistent success/error formats
4. **Pagination**: Limit/offset for list endpoints
5. **Filtering/Sorting**: Query parameters for flexibility

### API Response Format
```typescript
// Success Response
{
  "success": true,
  "data": { /* response data */ },
  "meta": { /* pagination, etc. */ }
}

// Error Response
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [ /* field-specific errors */ ]
  }
}
````

## 🗄️ Database Architecture

### PostgreSQL Schema Overview

```sql
-- Core Tables
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(255),
  password_hash VARCHAR(255) NOT NULL,
  avatar_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE workspaces (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  slug VARCHAR(255) UNIQUE NOT NULL,
  description TEXT,
  owner_id UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE workspace_members (
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
  role VARCHAR(50) DEFAULT 'member',
  joined_at TIMESTAMP DEFAULT NOW(),
  PRIMARY KEY (user_id, workspace_id)
);

CREATE TABLE projects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  description TEXT,
  workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
  status VARCHAR(50) DEFAULT 'active',
  due_date TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE tasks (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title VARCHAR(255) NOT NULL,
  description TEXT,
  project_id UUID REFERENCES projects(id) ON DELETE CASCADE,
  assignee_id UUID REFERENCES users(id) ON DELETE SET NULL,
  status VARCHAR(50) DEFAULT 'todo',
  priority VARCHAR(50) DEFAULT 'medium',
  due_date TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE files (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  task_id UUID REFERENCES tasks(id) ON DELETE CASCADE,
  filename VARCHAR(255) NOT NULL,
  original_name VARCHAR(255) NOT NULL,
  url TEXT NOT NULL,
  mime_type VARCHAR(100),
  size BIGINT,
  version INTEGER DEFAULT 1,
  uploaded_by UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE comments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  content TEXT NOT NULL,
  task_id UUID REFERENCES tasks(id) ON DELETE CASCADE,
  file_id UUID REFERENCES files(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  timestamp VARCHAR(20), -- For video comments "1:23"
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Database Design Decisions

1. **UUID Primary Keys**: Better than sequential IDs for distributed systems
2. **Soft Deletes**: `deleted_at` column instead of hard deletes
3. **Index Strategy**: Index foreign keys and frequently queried columns
4. **Partitioning**: Large tables partitioned by `created_at` date
5. **Full-Text Search**: PostgreSQL native search for comments/content

## ☁️ Infrastructure & Deployment

### Development Environment

```
Local Development
├── Frontend: localhost:3000 (Vite dev server)
├── Backend: localhost:3001 (Node.js + Nodemon)
├── Database: PostgreSQL local or Docker
└── Storage: Local disk or MinIO (S3-compatible)
```

### Production Environment

```
Production Stack
├── Frontend: Vercel (Global CDN, auto deployments)
├── Backend: Railway/Render (Node.js hosting)
├── Database: Railway PostgreSQL (managed)
├── Cache: Railway Redis (optional)
├── Storage: Cloudflare R2 (S3-compatible, cheaper)
├── CDN: Cloudflare (DDoS protection, caching)
└── Monitoring: Sentry + Logtail
```

### Environment Variables

```bash
# Backend (.env)
DATABASE_URL="postgresql://user:pass@localhost:5432/creatorflow"
REDIS_URL="redis://localhost:6379"
JWT_SECRET="your-super-secret-jwt-key-here"
S3_ENDPOINT="https://r2.cloudflarestorage.com"
S3_ACCESS_KEY="your-access-key"
S3_SECRET_KEY="your-secret-key"
S3_BUCKET="creatorflow"
NODE_ENV="production"
PORT="3001"
CORS_ORIGIN="https://app.creatorflow.com"

# Frontend (.env)
VITE_API_URL="https://api.creatorflow.com"
VITE_APP_URL="https://app.creatorflow.com"
VITE_SENTRY_DSN="your-sentry-dsn"
```

## 🔐 Security Architecture

### Authentication & Authorization

1. **JWT Tokens**: Stateless authentication, 7-day expiry
2. **Refresh Tokens**: Secure token rotation
3. **Role-Based Access Control (RBAC)**:
   - Owner: Full access
   - Admin: Manage users, settings
   - Member: Create/edit content
   - Guest: View only
4. **API Keys**: For third-party integrations (future)

### Data Protection

1. **Encryption at Rest**: Database encryption enabled
2. **Encryption in Transit**: TLS 1.3 for all communications
3. **Secure File Storage**: S3 server-side encryption
4. **Password Security**: bcrypt with 12 rounds

### API Security

1. **Rate Limiting**: 100 req/min per IP, 1000 req/hour per user
2. **CORS**: Strict origin validation
3. **Input Validation**: Zod schemas for all inputs
4. **SQL Injection Protection**: Prisma parameterized queries
5. **XSS Protection**: Content Security Policy headers

## 📈 Scalability Considerations

### Horizontal Scaling

```yaml
# Backend scaling (Docker/Kubernetes example)
replicas: 3
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
autoscaling:
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilization: 70
```

### Database Scaling

1. **Connection Pooling**: PgBouncer for connection management
2. **Read Replicas**: For heavy read workloads
3. **Database Indexing**: Comprehensive index strategy
4. **Query Optimization**: Regular query analysis

### File Storage Scaling

1. **CDN Integration**: Cloudflare CDN for static assets
2. **Multi-Region**: R2 multi-region replication (future)
3. **Lifecycle Policies**: Automatic archival of old files

## 🔄 Data Flow Examples

### User Registration Flow

```
1. Frontend → POST /api/v1/auth/register
2. Backend → Validate input (Zod)
3. Backend → Hash password (bcrypt)
4. Backend → Create user (Prisma)
5. Backend → Generate JWT
6. Backend → Return user + token
7. Frontend → Store token, redirect to dashboard
```

### File Upload Flow

```
1. Frontend → POST /api/v1/files/upload-url
2. Backend → Generate pre-signed S3 URL
3. Frontend → Upload directly to S3 using URL
4. Frontend → POST /api/v1/files with metadata
5. Backend → Create file record in database
6. Backend → Return file object to frontend
```

### Real-time Collaboration (Future)

```
1. User A edits task → Frontend sends PATCH /api/v1/tasks/:id
2. Backend updates database
3. Backend publishes event to Redis Pub/Sub
4. Other users' WebSocket connections receive update
5. Frontend updates UI in real-time
```

## 🧪 Testing Strategy

### Test Pyramid

```
        End-to-End (10%)
           │
    Integration (20%)
           │
       Unit (70%)
```

### Test Tools

- **Unit Tests**: Jest + React Testing Library
- **Integration Tests**: Supertest + Jest
- **E2E Tests**: Playwright/Cypress
- **API Tests**: Postman/Newman collections
- **Performance Tests**: k6/Lighthouse

## 📊 Monitoring & Observability

### Key Metrics

- **Application**: Request rate, error rate, latency (p95, p99)
- **Database**: Query performance, connection pool usage
- **Infrastructure**: CPU, memory, disk I/O
- **Business**: Active users, workspaces created, tasks completed

### Monitoring Tools

1. **Application Performance**: Sentry
2. **Logging**: Pino + Logtail
3. **Infrastructure**: Railway/Render built-in metrics
4. **Uptime**: Pingdom/StatusCake
5. **Analytics**: Plausible (privacy-friendly)

---

## 🔗 Related Documents

- [API Documentation](api.md) - Detailed endpoint specifications
- [Development Guide](DEVELOPMENT_GUIDE.md) - How to set up and code
- [Deployment Guide](DEPLOYMENT_GUIDE.md) - Production deployment
- [Database Schema](database-schema.md) - Complete Prisma schema

## 📝 Revision History

| Version | Date       | Changes                       | Author           |
| ------- | ---------- | ----------------------------- | ---------------- |
| 1.0     | 2024-01-15 | Initial architecture document | CreatorFlow Team |
