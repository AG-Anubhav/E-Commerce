# ShopVerse — Local Setup Guide

This guide walks you through running ShopVerse on your own machine.

---

## Prerequisites

You need two things installed:

1. **Node.js 20 or higher** — [Download here](https://nodejs.org/)
2. **PostgreSQL 14 or higher** — See installation instructions below

### Installing PostgreSQL

**Windows:**
1. Download the installer from [postgresql.org/download/windows](https://www.postgresql.org/download/windows/)
2. Run the installer and follow the prompts
3. Remember the password you set for the `postgres` user
4. The installer includes pgAdmin (a visual database tool) which can be helpful

**Mac:**
```bash
# Using Homebrew (recommended)
brew install postgresql@16
brew services start postgresql@16
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

## Step 1: Download the Project

Download or clone the project files to a folder on your machine.

---

## Step 2: Install Dependencies

Open a terminal in the project folder and run:

```bash
npm install
```

This installs all required packages.

---

## Step 3: Create a PostgreSQL Database

Open a terminal and connect to PostgreSQL:

```bash
# Windows: open "SQL Shell (psql)" from the Start menu, or run:
psql -U postgres

# Mac/Linux:
sudo -u postgres psql
```

Then create a database:

```sql
CREATE DATABASE shopverse;
```

Type `\q` to exit.

---

## Step 4: Configure Environment Variables

1. Copy the example environment file:

```bash
cp .env.example .env
```

2. Open `.env` in a text editor and fill in your values:

```
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5432/shopverse
SESSION_SECRET=any-random-string-you-like
```

Replace `YOUR_PASSWORD` with the password you set during PostgreSQL installation. On Mac/Linux with default peer authentication, you may use:

```
DATABASE_URL=postgresql://localhost:5432/shopverse
```

---

## Step 5: Set Up the Database Tables

Run this command to create all the required tables:

```bash
npm run db:push
```

You should see output confirming the tables were created.

---

## Step 6: Start the App (Development Mode)

```bash
npm run dev
```

The app will start and be available at **http://localhost:5000**

On first launch, the database is automatically seeded with sample categories and products.

---

## Step 7: Production Build (Optional)

If you want to run a production-optimized version:

```bash
# Build the app
npm run build

# Start in production mode
npm start
```

The production build serves pre-compiled files and runs faster.

---

## Using the App

1. Open **http://localhost:5000** in your browser
2. Browse products, add items to your cart
3. To log in, click the user icon and enter any 10-digit phone number
4. The OTP code will appear in the response (this is demo mode — no real SMS is sent)
5. Complete checkout with any UPI ID (payment is simulated)

---

## Troubleshooting

### "Connection refused" or database errors
- Make sure PostgreSQL is running:
  - Windows: Check Services app for "postgresql" service
  - Mac: `brew services list` — look for postgresql
  - Linux: `sudo systemctl status postgresql`
- Double-check your `DATABASE_URL` in the `.env` file (username, password, database name)

### Port 5000 is already in use
- Add `PORT=3000` (or any free port) to your `.env` file

### "relation does not exist" errors
- Run `npm run db:push` to create the database tables

### No products showing up
- The database seeds automatically on first run in development mode
- If products are missing, restart the app with `npm run dev`

### Permission denied (Mac/Linux PostgreSQL)
- Try connecting with: `psql -U postgres -h localhost`
- You may need to edit `pg_hba.conf` to allow password authentication for localhost

---

## Notes for Real Production Use

This app runs in **demo mode** with some simplifications. For a real production deployment, you would want to:

1. **Real SMS for OTP**: Integrate a service like Twilio to send actual SMS codes instead of returning them in the API response
2. **Real Payment Gateway**: Replace the simulated UPI payment with a real provider like Razorpay or Stripe
3. **HTTPS**: Use a reverse proxy like Nginx with an SSL certificate
4. **Stronger Session Secret**: Use a long, randomly generated string (e.g., `openssl rand -hex 32`)
5. **Rate Limiting**: Add rate limiting to the OTP and login endpoints to prevent abuse
6. **Image Hosting**: Move product images to a CDN or cloud storage service instead of external URLs
