# Requirements Checklist – iDurar ERP CRM

This document lists all the dependencies and setup required to run the iDurar ERP CRM project in my local environment.

---

## 1. System & OS

- [x] **Operating System**
  - Requirement: Ubuntu / WSL2 (Linux environment)
  - My setup: WSL Ubuntu on Windows
  - Install (WSL Ubuntu on Windows):
    - Enable WSL and install Ubuntu from Microsoft Store.

---

## 2. Global Tools / Software

- [x] **Git**
  - Used to clone the repository.
  - Check if installed:
    ```bash
    git --version
    ```
  - Install on Ubuntu:
    ```bash
    sudo apt update
    sudo apt install -y git
    ```

- [x] **Node.js (LTS – v18+ / v20) & npm**
  - Required to run backend and frontend (Node & npm based).
  - My setup: Node.js v20.19.6 (includes npm).
  - Check versions:
    ```bash
    node -v
    npm -v
    ```
  - Install via NodeSource (example):
    ```bash
    # For Node 20 (LTS)
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt install -y nodejs
    ```

- [x] **Code Editor**
  - Example: VS Code (for editing `.env`, code, etc.).
  - Optional but recommended.

- [x] **Web Browser**
  - To access the frontend at `http://localhost:3000`.

---

## 3. External Service – MongoDB Atlas

- [x] **MongoDB Atlas Account**
  - Go to MongoDB Atlas website and create an account.

- [x] **Atlas Cluster**
  - Create a free-tier cluster (any valid name, e.g., `idurar-cluster`).
  - No local MongoDB installation required – using **MongoDB Atlas** only.

- [x] **Database User**
  - Create a database user in Atlas with:
    - Username: e.g. `idurar_user`
    - Password: e.g. `Idurar12345`
  - Give it proper read/write permissions.

- [x] **Network Access / IP Whitelist**
  - In MongoDB Atlas → **Network Access**:
    - Add **Current IP Address** so my local machine can connect.
    - Note: Must be updated whenever my IP changes.

- [x] **Connection URI**
  - From Atlas → `Connect` → `Connect your application`.
  - Example URI:
    ```text
    mongodb+srv://idurar_user:Idurar12345@idurar-cluster.n5utrbg.mongodb.net/idurar
    ```
  - This URI is used in the backend `.env` file.

---

## 4. Project Repository Setup

- [x] **Clone the Repository**
  - Commands:
    ```bash
    git clone https://github.com/idurar/idurar-erp-crm.git
    cd idurar-erp-crm
    ```

- [x] **Backend Environment File**
  - Location: `backend/.env`
  - Variable used so far:
    ```env
    DATABASE="mongodb+srv://idurar_user:Idurar12345@idurar-cluster.n5utrbg.mongodb.net/idurar"
    ```
  - Purpose: Tells the backend where the MongoDB Atlas database is.

---

## 5. Backend Dependencies (Project-Level)

> These are installed via `npm install` inside the backend folder.

- [x] **Install backend node_modules**
  - Commands:
    ```bash
    cd backend
    npm install
    ```
  - This installs all backend dependencies from `backend/package.json`.

- [x] **Run setup script**
  - Command:
    ```bash
    npm run setup
    ```
  - Result (what I got):
    - Admin created
    - Settings created
    - Taxes created
    - PaymentMode created
    - Setup completed: Success ✅

- [x] **Run backend server**
  - Command:
    ```bash
    npm run dev
    ```
  - Output:
    ```text
    Express running → On PORT : 8888
    ```

---

## 6. Frontend Dependencies (Project-Level)

> These are installed via `npm install` inside the frontend folder.

- [x] **Install frontend node_modules**
  - Commands:
    ```bash
    cd ../frontend
    npm install
    ```

- [x] **Run frontend server**
  - Command:
    ```bash
    npm run dev
    ```
  - Access in browser:
    - `http://localhost:3000`

---

## 7. Known Node/OpenSSL Issue (If Using Node 17)

> (Not needed for me now because I’m using Node 20, but documenting as per project docs.)

- [ ] If using Node v17 and get OpenSSL error:
  - Option A (Recommended): Upgrade to Node v18 or v20.
  - Option B: Enable legacy OpenSSL provider:
    - On Linux/macOS:
      ```bash
      export NODE_OPTIONS=--openssl-legacy-provider
      npm run dev
      ```
    - On Windows CMD:
      ```cmd
      set NODE_OPTIONS=--openssl-legacy-provider
      npm run dev
      ```
    - On PowerShell:
      ```powershell
      $env:NODE_OPTIONS = "--openssl-legacy-provider"
      npm run dev
      ```

---

## Status

- [x] Backend connected to MongoDB Atlas (setup succeeded).
- [x] Backend running on `http://localhost:8888`.
- [x] Frontend running on `http://localhost:3000`.
- [x] No local MongoDB server required (Atlas used instead).


This covers everything you’ve already done for backend + frontend + NGINX, step-by-step, in proper DevOps language (interview + real-world friendly).

✅ Docker Containerization Checklist
Project: IDURAR ERP CRM

Task 2 – Containerization with Docker

✅ 1. Backend Containerization (Node.js + Express + MongoDB Atlas)
1.1 Analyze backend requirements

✅ Node.js version: 20.x

✅ Entry point: npm start

✅ Server file: src/server.js

✅ Port exposed by app: 8888

✅ Database: MongoDB Atlas (cloud)

✅ Environment variables required:

DATABASE

JWT_SECRET

NODE_ENV

PUBLIC_SERVER_FILE

1.2 Backend Dockerfile (Multi-Stage)

✅ Implemented multi-stage build to reduce image size

Build stage

Used lightweight base image: node:20-alpine

Copied only package.json and package-lock.json first (layer caching)

Installed dependencies

Copied full source code

Runtime stage

Used minimal base image: node:20-alpine

Created non-root user (appuser)

Copied app from build stage

Installed only production dependencies (npm install --omit=dev)

Exposed application port 8888

Ran app as non-root user

Started server using npm start

✅ Security best practices applied

Non-root user

No secrets baked into image

Minimal base image

Correct layer ordering

1.3 Backend Image Build
docker build -t idurar-backend:1.0.0 .


✅ Image successfully built
✅ Size optimized using Alpine + multi-stage

1.4 Backend Container Run
docker run -d \
  --name idurar-backend \
  -p 8888:8888 \
  -e DATABASE="mongodb+srv://idurar_user:Idurar12345@idurar-cluster.n5utrbg.mongodb.net/idurar?retryWrites=true&w=majority" \
  -e JWT_SECRET="your_private_jwt_secret_key" \
  -e NODE_ENV=production \
  -e PUBLIC_SERVER_FILE="http://localhost:8888/" \
  idurar-backend:1.0.0


✅ Backend running successfully
✅ Connected to MongoDB Atlas
✅ Auth working correctly
✅ API reachable on http://localhost:8888

1.5 Backend Verification
docker ps
docker logs idurar-backend
curl http://localhost:8888/api


✅ Express server started
✅ API responds correctly
✅ JWT error resolved by proper env injection

✅ 2. Frontend Containerization (React + Vite + NGINX)
2.1 Analyze frontend requirements

✅ Build tool: Vite

✅ Build output directory: dist/

✅ Runtime: NGINX

✅ SPA routing required (try_files)

✅ Frontend served on port 3000 (host)

2.2 Frontend Dockerfile (Multi-Stage)

✅ Implemented build stage + runtime stage

Build stage

Base image: node:20-alpine

Installed dependencies

Built Vite app using npm run build

Output generated in /app/dist

Runtime stage

Base image: nginx:1.27-alpine

Removed default NGINX config

Added custom nginx.conf

Copied static build files to /usr/share/nginx/html

Exposed port 80

Ran NGINX in foreground

2.3 NGINX Configuration (SPA-friendly)

✅ Custom nginx.conf

server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /assets/ {
        try_files $uri =404;
    }

    gzip on;
    gzip_types text/plain text/css application/json application/javascript application/xml+rss application/xml image/svg+xml;
}


✅ Supports React SPA routing
✅ Proper static asset handling
✅ Compression enabled

2.4 Frontend Image Build
docker build -t idurar-frontend:1.0.0 .


✅ Build successful
✅ Static assets generated
✅ NGINX correctly serving files

2.5 Frontend Container Run
docker run -d \
  --name idurar-frontend \
  -p 3000:80 \
  idurar-frontend:1.0.0


✅ Frontend accessible at http://localhost:3000
✅ React app loads correctly
✅ Static assets served via NGINX

2.6 Frontend Verification
docker ps
docker logs idurar-frontend
curl http://localhost:3000


✅ NGINX running
✅ Assets loading successfully
✅ SPA routing works (/login, /dashboard, etc.)

✅ 3. Environment & Configuration Handling

✅ Secrets NOT hardcoded into Docker image

✅ Environment variables injected at runtime

✅ .env used only for local development

✅ .env.production not required

✅ Docker overrides env values correctly

✅ 4. Docker Best Practices Applied

✅ Multi-stage builds
✅ Minimal Alpine images
✅ Non-root user (backend)
✅ Proper layer caching
✅ No secrets inside images
✅ Clean NGINX config
✅ Separation of frontend & backend containers

✅ 5. Current Architecture (Docker)
Browser
   |
   |── http://localhost:3000  → Frontend (NGINX + React)
   |
   |── http://localhost:8888  → Backend (Node.js + Express)
                                  |
                                  └── MongoDB Atlas (Cloud)


### Task 2 – Docker Compose (Backend + Frontend + Network)

- [x] Created **multi-stage Dockerfile (backend)** using `node:20-alpine`
  - Build stage installs dependencies
  - Runtime stage runs app with `npm start`
  - Uses non-root user (`appuser`)
  - Exposes port `8888`
- [x] Created **multi-stage Dockerfile (frontend)** using:
  - Build stage: `node:20-alpine` + `npm run build` (Vite)
  - Runtime stage: `nginx:1.27-alpine` serving static files from `/usr/share/nginx/html`
  - Custom `nginx.conf` to serve SPA build on port `80`
  - Build ARG `VITE_BACKEND_SERVER` to configure API base URL

- [x] Added `.dockerignore` (backend & frontend) to speed up builds:
  - `node_modules/`
  - `dist/` / `build/`
  - `.git`, `logs`, `Dockerfile`, etc.

- [x] Created **`docker-compose.yml`** at project root:
  - `backend` service:
    - Build from `./backend`
    - Ports: `8888:8888`
    - Uses `backend/.env` for `DATABASE`, `JWT_SECRET`, etc.
    - `NODE_ENV=production`, `PUBLIC_SERVER_FILE=http://localhost:8888/`
  - `frontend` service:
    - Build from `./frontend`
    - Passes `VITE_BACKEND_SERVER="http://localhost:8888/"` as build ARG
    - Ports: `3000:80`
    - Depends on `backend`

- [x] Configured **Docker network**:
  - Defined custom bridge network `idurar-net`
  - Both services (`backend`, `frontend`) attached to `idurar-net`

- [x] **Database & persistence:**
  - Application uses **MongoDB Atlas (cloud)** instead of a local MongoDB container
  - Persistence is handled by Atlas; no local Docker volume required
  - DB connection configured via `DATABASE` URI in `backend/.env`

- [x] Validated end-to-end:
  - `http://localhost:3000` → frontend served by Nginx container
  - `http://localhost:8888/api` → backend API running in Node container
  - Login and dashboard flows working against MongoDB Atlas