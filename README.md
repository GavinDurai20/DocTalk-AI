# 🩺 DocTalk AI

> DocTalk AI is an AI-powered medical voice assistant built with Next.js that enables real-time voice conversations, AI-generated medical reports, and doctor recommendations, with a production-ready Docker and CI/CD workflow.

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

- 🎙️ Real-time AI voice conversations with Vapi
- 🤖 AI-powered medical assistance with OpenRouter
- 📋 AI-generated medical reports
- 👨‍⚕️ Doctor and specialist recommendations
- 🔐 Authentication with Clerk
- 🗄️ PostgreSQL persistence with Neon and Drizzle ORM
- 💬 Medical session and conversation history
- 📱 Responsive desktop and mobile UI

---

## 📊 Engineering Highlights

- **Next.js 15.3.4** production application
- **React 19** with TypeScript
- **13/13 pages** successfully generated during production build
- Multi-stage production Docker build
- Docker image reduced from **~1.81 GB to ~310.6 MB**
- **~83% container image size reduction**
- Production container runs as a **non-root user**
- Automated **GitHub Actions CI/CD**
- Container publishing to **GitHub Container Registry (GHCR)**
- Production deployment through **Vercel**

---

## 🏗️ Architecture

```text
                                      ┌──────────────────┐
                                      │       USER       │
                                      │ Browser / Mobile │
                                      └────────┬─────────┘
                                               │
                                               ▼
                              ┌────────────────────────────────┐
                              │       NEXT.JS APPLICATION       │
                              │ Next.js 15 · React 19 · TS     │
                              └───────────────┬────────────────┘
                                              │
                    ┌─────────────────────────┼─────────────────────────┐
                    │                         │                         │
                    ▼                         ▼                         ▼
              ┌───────────┐             ┌───────────┐            ┌────────────┐
              │   Clerk   │             │   Vapi    │            │ OpenRouter │
              │   Auth    │             │ Voice AI  │            │     AI     │
              └───────────┘             └───────────┘            └────────────┘
                                              │                         │
                                              └──────────┬──────────────┘
                                                         ▼
                                              ┌────────────────────┐
                                              │  Next.js API Routes│
                                              └──────────┬─────────┘
                                                         │
                                                         ▼
                                              ┌────────────────────┐
                                              │    Drizzle ORM     │
                                              └──────────┬─────────┘
                                                         │
                                                         ▼
                                              ┌────────────────────┐
                                              │  Neon PostgreSQL   │
                                              └────────────────────┘


                         ─────────────── DELIVERY PIPELINE ───────────────

       Developer
           │
           ▼
        GitHub
           │
           ▼
    ┌─────────────────┐
    │ GitHub Actions  │
    │                 │
    │ npm ci           │
    │ Next.js build    │
    │ Docker build     │
    │ Push image       │
    └────────┬────────┘
             │
             ▼
            GHCR
             │
             │
             └──────────────────────┐
                                    │
                                    ▼
                                  Vercel
                                    │
                                    ▼
                            Production Application
```

---

## 🐳 Docker

The application uses a **multi-stage Docker build** with Next.js standalone output.

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

The production image contains only the required runtime artifacts and runs as a non-root user.

---

## ⚙️ CI/CD + GHCR

GitHub Actions automates:

1. Dependency installation with `npm ci`
2. Next.js production build
3. Docker image build
4. Container publishing to GHCR

The workflow runs on pushes to `main` and pull requests targeting `main`.

```text
GitHub
   │
   ▼
GitHub Actions
   │
   ├── Next.js Build
   └── Docker Build
           │
           ▼
          GHCR
```

---

## ☁️ Deployment

**Vercel** is used for production application hosting.

**GHCR** stores the reproducible Docker container image produced by the CI pipeline.

```text
GitHub ──► GitHub Actions ──► GHCR
   │
   └────────────────────────► Vercel
```

---

## 🔐 Security

- `.env.local` is excluded from source control
- Sensitive credentials are managed outside Git
- GitHub Actions uses repository secrets
- Private credentials are not passed as Docker build arguments
- Runtime environment variables are used for sensitive configuration
- Production containers run as a non-root user

Example environment configuration:

```env
DATABASE_URL=
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
OPEN_ROUTER_API_KEY=
NEXT_PUBLIC_VAPI_VOICE_ASSISTANT_ID=
NEXT_PUBLIC_API_KEY=
```

> Never commit `.env.local` or private API credentials to the repository.

---

## 👨‍💻 Author

**Gavin Durai**

[GitHub](https://github.com/GavinDurai20)
