# ShareScreenly — Screen recording & instant video sharing

![TypeScript](https://img.shields.io/badge/TypeScript-5+-3178C6?logo=typescript)
![Next.js](https://img.shields.io/badge/Next.js-15-000000?logo=nextdotjs)
![Node.js](https://img.shields.io/badge/Node.js-20+-43853D?logo=nodedotjs)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3+-06B6D4?logo=tailwindcss)
![Better Auth](https://img.shields.io/badge/Better_Auth-Auth%20%2B%20RBAC-111827)
![Drizzle ORM](https://img.shields.io/badge/Drizzle-ORM-0F766E)
![Xata](https://img.shields.io/badge/Xata-Serverless_Postgres-6E59A5)
![Bunny.net](https://img.shields.io/badge/Bunny.net-Stream_CDN-F97316)
![FFmpeg](https://img.shields.io/badge/FFmpeg-Transcode-007808?logo=ffmpeg)
![Arcjet](https://img.shields.io/badge/Arcjet-Rate_Limits_%26_Bot_Protection-3B82F6)

> Adopted by **100+ SFU students** for demo sharing, cutting setup from **5 min → 30 sec**.  
> Scalable uploads, instant links, privacy controls, AI transcripts, and search.

---

## 📌 Overview

**ShareScreenly** is a full-stack platform to **record your screen**, **upload**, and **share** videos instantly.  
It uses **Bunny.net** for streaming/CDN, **FFmpeg** for compression & metadata, **Better Auth** for secure login and **role-based access**, and a **Postgres** backend (via **Xata + Drizzle**). Optional protections (rate limits, bot checks) come from **Arcjet**.

---

## ⚙️ Tech Stack

- **Frontend:** Next.js 15 (App Router), TypeScript, Tailwind CSS  
- **Auth:** Better Auth (email/Google), role-based access  
- **Storage/Streaming:** Bunny Stream (+ edge delivery)  
- **Database:** Xata (serverless Postgres) + Drizzle ORM (migrations, types)  
- **Video Processing:** FFmpeg (compression, thumbnails, duration, codecs)  
- **Security:** Arcjet (bot protection, rate limiting, email validation)

> Prefer AWS? Swap Bunny for **S3 + CloudFront** with minimal changes (upload service abstraction).

---

## 🔋 Features

- **Auth & RBAC:** Better Auth with email/Google and role-aware routes  
- **Screen Recorder:** Record tab/window/screen in-app, no extensions needed  
- **Fast Uploads:** Resumable uploads to Bunny; serverless ingest webhooks  
- **FFmpeg Pipeline:** Compress, extract metadata (duration, bitrate), generate thumbnails  
- **Privacy Controls:** Public/unlisted/private + expiring share links  
- **AI Transcripts:** Auto-transcribe; searchable captions (optional)  
- **Search & Filters:** Title, tags, owner, visibility, duration  
- **Dashboard:** Recent videos, processing state, quick share, delete/restore  
- **Responsive UI:** Tailwind + shadcn/ui components, dark-mode ready  
- **Secure by default:** Arcjet rate limits on auth/upload/api routes

---

## Architecture (high level)
```bash
Client (Next.js + Recorder)
   │
   ├─ Upload → Bunny Stream  ──► Webhook → API Route  ──► FFmpeg worker (edge/job)
   │                                    │
   │                                    └─► Drizzle (Xata Postgres): videos, metadata, transcripts
   │
   └─ Playback ← Bunny CDN  ──► Signed/Tokenized HLS/DASH
ARCH
```


## Quick Start

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)

**Cloning the Repository**

```bash
git clone https://github.com/mehrdadmmz/ShareScreenly
cd ShareScreenly
```

**Installation**

Install the project dependencies using npm:

```bash
npm install
```

**Set Up Environment Variables**

Create a new file named `.env` in the root of your project and add the following content:

```env
# Next.js
NEXT_PUBLIC_BASE_URL=http://localhost:3000

# [Xata] Configuration used by the CLI and the SDK
# Make sure your framework/tooling loads this file on startup to have it available for the SDK
XATA_API_KEY=
DATABASE_URL_POSTGRES=

# Google
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

# BetterAuth
BETTER_AUTH_SECRET=
BETTER_AUTH_URL=http://localhost:3000

# Bunny
BUNNY_STORAGE_ACCESS_KEY=
BUNNY_LIBRARY_ID=
BUNNY_STREAM_ACCESS_KEY=

#ArcJet
ARCJET_API_KEY=
XATA_API_KEY=
```

Replace the placeholder values with your actual credentials. You can obtain these credentials by signing up on: [Better-Auth](https://www.better-auth.com), [Google Cloud](https://console.cloud.google.com), [Bunny.net](https://jsm.dev/snapcast-bunny), [Xata.io](https://xata.io), [Arcjet](https://jsm.dev/snapcast-arcjet).

**Running the Project**

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the project.

## Suggested Structure
```bash
/app
  /(auth)            # sign-in/up
  /(root)
    /dashboard
    /video/[id]
  /api
    /upload          # signed upload / ingest
    /webhooks/bunny  # processing callbacks
/lib
  /auth              # Better Auth setup
  /db                # Drizzle config & schema
  /video             # FFmpeg, metadata, thumbnails
  /security          # Arcjet guards
/components         # UI (shadcn/ui wrappers)

```

## Security Notes

- Better Auth sessions are HTTP-only; protect API routes with server checks.
- Arcjet rate limits on /api/upload, /api/webhooks/*, auth endpoints.
- Tokenized playback/links for private videos.
- Validate all webhook signatures.

## Tips & Ops

- FFmpeg: use hardware-friendly H.264 + AAC for widest compatibility.
- Bunny: enable token auth and signed URLs for private playback.
- DB: run Drizzle migrations on deploy; keep schema in lib/db/schema.ts.

  
