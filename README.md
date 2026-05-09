
---

## 📖 About

**InkFig** is an integrated, full-stack web application built as the official digital creative hub for Hebron University (جامعة الخليل). It solves a real gap in the university's academic environment: the complete absence of a formal, centralized platform for sharing, discovering, and celebrating creative works.

> *The name combines **Ink** — symbolizing creativity and artistic expression — and **Fig** — a nod to local Palestinian cultural identity.*

### The Problem

Before InkFig, Hebron University's creative community had no official home. Students' projects — digital art, animations, games, music — were scattered across personal Instagram pages, WhatsApp groups, and USB drives. The annual university multimedia exhibition was managed entirely by hand. Talented creators received no recognition. Intellectual property went unprotected.

### The Solution

InkFig provides a professional, secure, feature-rich multimedia gallery that serves as the permanent digital portfolio for every member of the university community — with a purpose-built system for managing the annual multimedia exhibition.

---

## ✨ Features

### 🗂️ Multimedia Gallery
- Browse, search, and filter a gallery of creative works across **9 media types**
- Smart feed ranked by **user preferences** and content type
- Sort by: newest, most popular, most liked, highest rated
- Works displayed as rich cards with title, thumbnail, author, likes, downloads, comments, and category

### 🎭 Supported Media Types

| Type | Description |
|------|-------------|
| 🖥️ Digital Art | Digital illustrations, graphic designs, edited images |
| ✏️ Hand Art | Scanned or photographed traditional artwork |
| 🎬 Video | Short films, documentaries, presentations |
| 🌀 Animation | 2D/3D animations and motion projects |
| 🎮 Game | Electronic games and game prototypes |
| 🥽 VR / AR | Immersive virtual and augmented reality experiences |
| 🎵 Audio | Music composition, podcasts, sound design, recordings |
| 🖱️ Interactive | HTML5 interactive experiences and creative web projects |

### 🏆 Annual Exhibition Feature
A signature feature of InkFig — a dedicated section for Hebron University's yearly multimedia exhibition:

- **Admin-controlled mode**: Exhibition is locked/unlocked by the admin
- **Submission flow**: Work owners submit inclusion requests → lecturers approve/reject with reasons
- **Exhibition badge**: Accepted works display a visible "مدرج في المعرض" (In Exhibition) badge
- **Star ratings**: Activated exclusively on exhibition works during the event period
- **Leaderboard**: Works ranked by final ratings; top works announced at event end
- **Announcement slider**: Appears on the homepage when exhibition mode is active

### 👤 User Profiles
- Public portfolio page for every member
- 3-column work grid with follower/like statistics
- Follow system between community members

### 🔐 Security & Access Control
- **5-level role system**: System Admin → Admin → Lecturer → Student → Visitor
- University email verification (only `@hebron.edu` addresses)
- Content moderation approval workflow before publishing
- **Automatic watermarking**: Platform logo applied transparently to all downloaded images — original files untouched
- Full data encryption

### 🔔 Social & Community
- Likes, comments (with replies), and star ratings
- Real-time notifications
- Follow/following system
- Download tracking

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | [Next.js](https://nextjs.org/) — React framework with SSR/SSG, full Arabic/RTL support |
| **Backend** | [FastAPI](https://fastapi.tiangolo.com/) — Python REST API, async, auto-documented (Swagger/OpenAPI) |
| **Database** | Relational database (PostgreSQL) via SQLAlchemy ORM |
| **File Storage** | [AWS S3](https://aws.amazon.com/s3/) — scalable object storage for all media files |
| **Auth** | JWT-based authentication with email verification |
| **Design (Wireframes)** | Figma |
| **Methodology** | Agile — iterative development |

### Why This Stack?

- **Next.js** — Built-in routing, server-side rendering for SEO, image optimization, and first-class RTL/Arabic support via CSS
- **FastAPI** — Blazing fast Python backend with automatic OpenAPI docs, native async support, and Pydantic data validation
- **AWS S3** — Industry-standard object storage; handles video, audio, images at any scale with pre-signed URLs for secure direct uploads

---

## 🚀 Getting Started

### Prerequisites

- Node.js >= 18
- Python >= 3.11
- AWS account with an S3 bucket configured
- PostgreSQL database

---

### 🖥️ Frontend (Next.js)

```bash
# Navigate to frontend
cd frontend

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Fill in: NEXT_PUBLIC_API_URL, NEXT_PUBLIC_AWS_REGION, etc.

# Run the development server
npm run dev
# → http://localhost:3000
```

---

### ⚙️ Backend (FastAPI)

```bash
# Navigate to backend
cd backend

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Fill in: DATABASE_URL, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_S3_BUCKET, SECRET_KEY, etc.

# Run database migrations
alembic upgrade head

# Start the development server
uvicorn main:app --reload
# → http://localhost:8000
# → Swagger docs: http://localhost:8000/docs
```

---

### ☁️ AWS S3 Setup

```bash
# Configure AWS CLI
aws configure
# Enter: AWS Access Key ID, Secret Access Key, Region (e.g. me-south-1)

# Create S3 bucket
aws s3 mb s3://inkfig-media

# Set CORS policy (required for direct browser uploads)
aws s3api put-bucket-cors --bucket inkfig-media --cors-configuration file://s3-cors.json
```

**`s3-cors.json`** example:
```json
{
  "CORSRules": [
    {
      "AllowedOrigins": ["http://localhost:3000", "https://inkfig.hebron.edu"],
      "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
      "AllowedHeaders": ["*"],
      "MaxAgeSeconds": 3000
    }
  ]
}
```

---

### 🔑 Environment Variables

**Frontend `.env.local`**
```env
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
NEXT_PUBLIC_AWS_REGION=me-south-1
NEXT_PUBLIC_S3_BUCKET=inkfig-media
```

**Backend `.env`**
```env
DATABASE_URL=postgresql://user:password@localhost:5432/inkfig_db
SECRET_KEY=your-jwt-secret-key
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_S3_BUCKET=inkfig-media
AWS_REGION=me-south-1
MAIL_USERNAME=your-email@hebron.edu
MAIL_PASSWORD=your-email-password
```

---

## 📁 Project Structure

```
inkfig/
├── frontend/                  # Next.js application
│   ├── app/                   # App Router (Next.js 14+)
│   │   ├── (auth)/            # Login / Register pages
│   │   ├── gallery/           # Main media gallery
│   │   ├── exhibition/        # Annual exhibition section
│   │   ├── profile/[id]/      # User profile pages
│   │   └── dashboard/         # Admin dashboard
│   ├── components/            # Reusable UI components
│   ├── lib/                   # API client, utilities
│   ├── public/                # Static assets
│   └── .env.local
│
├── backend/                   # FastAPI application
│   ├── app/
│   │   ├── api/               # Route handlers (v1)
│   │   │   ├── auth.py
│   │   │   ├── works.py
│   │   │   ├── exhibition.py
│   │   │   ├── users.py
│   │   │   └── admin.py
│   │   ├── models/            # SQLAlchemy models
│   │   ├── schemas/           # Pydantic schemas
│   │   ├── services/          # Business logic (S3, watermark, email)
│   │   └── core/              # Config, security, dependencies
│   ├── alembic/               # DB migrations
│   ├── requirements.txt
│   └── .env
│
└── s3-cors.json               # AWS S3 CORS policy
```

---



### System Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                          InkFig                                  │
├───────────────────┬──────────────────────┬───────────────────────┤
│   Frontend        │      Backend         │       Storage         │
│                   │                      │                       │
│  Next.js (React)  │  FastAPI (Python)    │  PostgreSQL (DB)      │
│  SSR + RTL/AR     │  RESTful API         │  AWS S3 (Media Files) │
│  Tailwind CSS     │  JWT Auth            │                       │
│  Port :3000       │  Swagger /docs       │                       │
│                   │  Port :8000          │                       │
└───────────────────┴──────────────────────┴───────────────────────┘

         Browser ──▶ Next.js ──▶ FastAPI ──▶ PostgreSQL
                                    │
                                    └──▶ AWS S3 (pre-signed URLs)
                                              ▲
                                    Browser ──┘  (direct upload)
```

### Database Tables (ERD Summary)

| Table | Purpose |
|-------|---------|
| `Users` | All members — name, email, role, faculty, specialization |
| `Works` | Uploaded works — title, type, category, file path, approval status |
| `Likes` | User ↔ Work like relationships |
| `Comments` | Comments and replies on works |
| `Ratings` | 1–5 star ratings (exhibition works only) |
| `Downloads` | Download tracking per work |
| `Follows` | User follow/following relationships |
| `Notifications` | Per-user notification store |
| `Exhibition` | Exhibition event settings — status, start/end dates |
| `ExhibitionRequests` | Submission requests — pending / accepted / rejected |

### Key Database Relationships
- **User → Works**: One-to-Many
- **Work → Likes / Comments / Ratings / Downloads**: One-to-Many
- **User ↔ User (Follows)**: Many-to-Many
- **Work → ExhibitionRequest**: One-to-One

---

## 📋 Functional Requirements Summary

### Member Operations
- ✅ Register with university email (activation via email link)
- ✅ Login / Logout
- ✅ Upload creative works (title, description, category, media type, file, thumbnail)
- ✅ Submit work for exhibition inclusion
- ✅ Like, comment, rate, and download works
- ✅ Follow/unfollow other members
- ✅ Receive real-time notifications

### Admin / Lecturer Operations
- ✅ Login to admin panel
- ✅ Toggle exhibition mode ON/OFF
- ✅ Review and approve/reject exhibition submission requests
- ✅ Moderate content and manage users
- ✅ View platform statistics dashboard

---

## 📊 Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| Response time | < 3 seconds (most operations) |
| Concurrent users | ≥ 5,000 simultaneous |
| Availability | 24/7 |
| Data security | Full encryption at rest and in transit |
| Accessibility | Full Arabic (RTL) support |
| Scalability | No degradation with growing data/users |

---

## 💰 Economic Feasibility

| Item | Cost (Annual) |
|------|--------------|
| Development | **0 NIS** — academic graduation project |
| Cloud hosting | 500 – 1,000 NIS |
| Domain name | 50 – 100 NIS |
| Maintenance (post-handover) | ~500 NIS |
| **Total** | **~1,050 – 1,600 NIS/year** |

Far more cost-effective than manual exhibition management (hall rentals, printed materials, manual sorting).

---

## 🗺️ Project Scope

| Scope | Detail |
|-------|--------|
| 🌍 Geographic | Hebron University |
| 👥 Users | Students, lecturers, staff, and interested visitors |
| 🎯 Domain | Multimedia portfolio platform + annual exhibition management |
| 📅 Timeline | Academic year 2025–2026 |

---

## 👥 Team

| Name | Role |
|------|------|
| محمد القواسمة | Team Member |
| أحمد أبو رعية | Team Member |
| أنس العطاونة | Team Member |
| أ. أمجد الحسيني | Academic Supervisor |

---

## 🏫 Institution


**Hebron University — جامعة الخليل**  
Faculty of Information Technology  
Hebron, Palestine  
Academic Year 2025 / 2026


---

## 📄 License

This project is developed as an academic graduation project at Hebron University. All rights reserved to the development team and the university.

---


Made with ❤️ at **Hebron University** · Faculty of Information Technology

*"Bringing Hebron University's creative community together — one work at a time."*

