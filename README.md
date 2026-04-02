# NisitDeeden — Nisit Deeden System

A comprehensive web application developed for **Kasetsart University** to manage and evaluate outstanding student awards. The project is modularized into three separate repositories — frontend, backend, and Docker infrastructure — and runs entirely in Docker for easy local development.

---

## Prerequisites

Make sure you have the following installed on your machine before getting started:

| Tool | Purpose |
|---|---|
| [Git](https://git-scm.com/) | Cloning the repositories |
| [Docker + Docker Compose](https://www.docker.com/) | Running everything — no PHP or Composer needed locally |

---

## Google OAuth requirement

> ⚠️ **Google OAuth is required** — this project uses Google Login as its only authentication method.
> You must create your own credentials at [console.cloud.google.com](https://console.cloud.google.com/apis/credentials) before running.
> Set the **Authorized redirect URI** to: `http://localhost/api/oauth2/callback/google`

---

## Step 1 — Clone repositories

```bash
git clone https://github.com/far555na/NisitDeeden-Webtech-Docker-Compose-Modularization.git
cd NisitDeeden-Webtech-Docker-Compose-Modularization

git clone https://github.com/far555na/NisitDeeden-Webtech-frontend.git
git clone https://github.com/MauriceZ48/NisitDeeden-Webtech-backend.git
```

---

## Step 2 — Configure environment files

```bash
cd NisitDeeden-Webtech-frontend
cp .env.example .env

cd ../NisitDeeden-Webtech-backend
cp .env.example .env

docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/app" \
    -w /app \
    composer:latest \
    install --ignore-platform-reqs
```

---

## Step 3 — Fill in .env values

The `.env.example` files already include working default values for `REVERB_*` — you just need to make sure the values **match** between backend and frontend `.env`. You can leave them as-is or change them to any string you like.

`APP_KEY` will be auto-generated in Step 4 — leave it blank for now.

The only values you **must** fill in manually are your Google credentials:

| Variable | How to get it |
|---|---|
| `GOOGLE_CLIENT_ID` + `VITE_GOOGLE_CLIENT_ID` | [Google Cloud Console](https://console.cloud.google.com/apis/credentials) |
| `GOOGLE_CLIENT_SECRET` | Same as above |

---

## Step 4 — Build and run

```bash
cd ..

chmod +x compose
./compose up -d --build

./compose exec laravel-api composer install
./compose exec laravel-api php artisan key:generate   # generates APP_KEY and writes it to .env automatically
./compose exec laravel-api php artisan migrate --seed
./compose exec laravel-api php artisan storage:link
```

---

## Done

Open **http://localhost:3000** in your browser.
