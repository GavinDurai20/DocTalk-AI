# 🩺 DocTalk AI

> AI-powered medical voice assistant built with Next.js for real-time voice conversations, medical assistance, AI-generated medical reports, and doctor recommendations.

[![CI](https://github.com/GavinDurai20/DocTalk-AI/actions/workflows/ci.yml/badge.svg)](https://github.com/GavinDurai20/DocTalk-AI/actions/workflows/ci.yml)
![Next.js](https://img.shields.io/badge/Next.js-15.3.4-black)
![React](https://img.shields.io/badge/React-19-61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6)
![Node.js](https://img.shields.io/badge/Node.js-20-339933)
![Docker](https://img.shields.io/badge/Docker-2496ED)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF)
![GHCR](https://img.shields.io/badge/GHCR-Container%20Registry-181717)

---

## 🚀 Features

- 🎙️ Real-time AI voice conversations using Vapi
- 🤖 AI-powered medical assistance using OpenRouter
- 📋 AI-generated medical reports
- 👨‍⚕️ Doctor and specialist recommendations
- 🔐 Authentication and user management with Clerk
- 🗄️ PostgreSQL persistence with Neon and Drizzle ORM
- 💬 Medical session and conversation history
- 📱 Responsive desktop and mobile interface
- 🐳 Production-ready Docker containerization
- ⚙️ Automated CI/CD with GitHub Actions
- 📦 Docker image publishing to GitHub Container Registry

---

## 📊 Engineering Highlights

- **Next.js 15.3.4** production application
- **React 19** with TypeScript
- **13/13 pages** successfully generated during production build
- **Multi-stage Docker** production build
- Docker image reduced from **~1.81 GB to ~310.6 MB**
- **~83% reduction** in container image size
- Production container runs as a **non-root user**
- Automated **GitHub Actions CI/CD**
- Automated **Docker image builds**
- Container publishing to **GitHub Container Registry (GHCR)**
- Production deployment through **Vercel**

---

## 🏗️ Architecture

```text
                                      ┌─────────────────────┐
                                      │        USER         │
                                      │  Browser / Mobile   │
                                      └──────────┬──────────┘
                                                 │
                                                 ▼
                              ┌─────────────────────────────────┐
                              │       NEXT.JS APPLICATION       │
                              │       Next.js 15 / React 19     │
                              │          TypeScript             │
                              └───────────────┬─────────────────┘
                                              │
                     ┌────────────────────────┼────────────────────────┐
                     │                        │                        │
                     ▼                        ▼                        ▼
              ┌─────────────┐          ┌─────────────┐          ┌─────────────┐
              │    Clerk    │          │    Vapi     │          │  OpenRouter │
              │    Auth     │          │   Voice AI  │          │     AI      │
              └─────────────┘          └─────────────┘          └─────────────┘
                                              │                        │
                                              └───────────┬────────────┘
                                                          │
                                                          ▼
                                              ┌─────────────────────┐
                                              │   Next.js API Routes│
                                              │ Medical Reports /   │
                                              │ Doctor Suggestions  │
                                              └──────────┬──────────┘
                                                         │
                                                         ▼
                                              ┌─────────────────────┐
                                              │     Drizzle ORM     │
                                              └──────────┬──────────┘
                                                         │
                                                         ▼
                                              ┌─────────────────────┐
                                              │   Neon PostgreSQL   │
                                              └─────────────────────┘


  ─────────────────────────────── DEVOPS DELIVERY ───────────────────────────────

          Developer
              │
              ▼
           Git / GitHub
              │
              ▼
      ┌──────────────────────┐
      │    GitHub Actions    │
      │                      │
      │  1. npm ci           │
      │  2. Next.js build    │
      │  3. Docker build     │
      │  4. Push container   │
      └──────────┬───────────┘
                 │
                 ▼
      ┌──────────────────────┐
      │        GHCR          │
      │ GitHub Container     │
      │      Registry        │
      └──────────────────────┘


  ─────────────────────────────── PRODUCTION ────────────────────────────────────

           GitHub
              │
              ├──────────────────────► Vercel
              │                           │
              │                           ▼
              │                    Production App
              │
              └──► GitHub Actions ───► GHCR
```

### Architecture Components

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | Next.js, React, TypeScript | Application UI and user experience |
| Authentication | Clerk | User authentication and management |
| Voice AI | Vapi | Real-time voice interaction |
| AI | OpenRouter / OpenAI SDK | AI-powered responses and report generation |
| API | Next.js API Routes | Server-side application logic |
| ORM | Drizzle ORM | Database access and queries |
| Database | Neon PostgreSQL | Persistent application data |
| Containerization | Docker | Reproducible production builds |
| CI/CD | GitHub Actions | Automated build and delivery |
| Registry | GHCR | Container image storage |
| Deployment | Vercel | Production application hosting |

---

## 🐳 Docker

DocTalk AI uses a **multi-stage Docker build** with Next.js standalone output.

### Docker Build

```text
Dependencies
     │
     ▼
   Builder
     │
     ├── npm ci
     ├── Copy source
     └── npm run build
             │
             ▼
       Production Image
             │
             ├── Next.js standalone
             ├── Static assets
             ├── Public assets
             └── Non-root user
```

### Build

```bash
docker build \
  --build-arg NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="your_key" \
  -t doctalk-ai .
```

### Run

```bash
docker run \
  --env-file .env.local \
  -p 3000:3000 \
  doctalk-ai
```

### Docker Compose

```bash
docker compose --env-file .env.local up --build
```

### Image Optimization

| Metric | Result |
|---|---:|
| Original image | ~1.81 GB |
| Optimized image | ~310.6 MB |
| Image reduction | **~83%** |

The production container runs as a **non-root user** and only includes the required production artifacts.

---

## ⚙️ CI/CD + GHCR

GitHub Actions automates the application build and Docker image delivery process.

### Pipeline

```text
Git Push / Pull Request
          │
          ▼
   GitHub Actions
          │
          ├── Checkout
          │
          ├── Setup Node.js 20
          │
          ├── npm ci
          │
          ├── Next.js production build
          │
          ├── Docker Buildx setup
          │
          ├── Docker image build
          │
          └── Push image
                  │
                  ▼
                 GHCR
```

The workflow runs for:

- Pushes to `main`
- Pull requests targeting `main`

### GHCR

Docker images are published to:

```text
GitHub Container Registry
        │
        ▼
    doctalk-ai
```

This provides a reproducible container artifact that can be pulled and run independently of the source repository.

---

## ☁️ Deployment

The production application is deployed through **Vercel**.

```text
                         GitHub
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
      GitHub Actions                 Vercel
              │                         │
              ▼                         ▼
             GHCR                Production App
```

- **Vercel** → production application hosting
- **GitHub Actions** → automated build and delivery
- **GHCR** → container image registry
- **Docker** → reproducible application packaging

---

## 🔐 Security

- `.env.local` is excluded from source control
- Sensitive credentials are stored outside Git
- GitHub Actions uses repository secrets
- Private credentials are not passed as Docker build arguments
- Runtime environment variables are used for sensitive configuration
- Production Docker containers run as a **non-root user**
- Client-exposed `NEXT_PUBLIC_*` variables are limited to public configuration

### Environment Variables

```env
DATABASE_URL=
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
OPEN_ROUTER_API_KEY=
NEXT_PUBLIC_VAPI_VOICE_ASSISTANT_ID=
NEXT_PUBLIC_API_KEY=
```

> Never commit `.env.local` or expose private API credentials in source control.

---

## 👨‍💻 Author

**Gavin Durai**

[GitHub](https://github.com/GavinDurai20)
