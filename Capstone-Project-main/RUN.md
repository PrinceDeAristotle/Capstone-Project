# How to run the project

## 1. Install dependencies

From this folder (the one with `package.json`):

```bash
pnpm install
```

## 2. Environment

Copy `.env.example` to `.env` and edit if needed.

- **PORT** 3000  
- **MONGODB_URI** – needed for login/register. Default: `mongodb://localhost:27017/quizpulse` (local MongoDB).  
  See **MONGODB_SETUP.md** for local install or free MongoDB Atlas setup.  
- **JWT_SECRET** – any string for signing tokens (use a long random value in production).

If MongoDB is not running or not set, the server still starts; login/register will fail until MongoDB is connected.

## 3. Start the app

```bash
pnpm run dev
```

Then open: **http://localhost:3000** (or the port shown in the terminal).

## 4. Teacher dashboard without logging in

If you open the teacher dashboard without being logged in, you’ll see:

- **“You need to sign in to view the teacher dashboard.”**
- A **“Go to Sign in”** button.

Click **“Go to Sign in”** to go to the login page.  
If the app seems stuck on loading, wait up to 8 seconds; then the same sign-in message and button will appear.

## 5. If `pnpm run dev` fails

- Run the terminal from this project folder (where `package.json` is).
- If you see `cross-env` not found, run `pnpm install` again.
- On Windows, avoid running the terminal as Administrator.

## Vercel (static frontend)

The repo includes [vercel.json](vercel.json) so you can deploy the **Vite client** build (`dist/public`) to Vercel: install/build commands, `outputDirectory`, and SPA rewrites for client-side routes.

**Important:** Vercel serves the **static UI** only. This app’s **Express API**, **tRPC** (`/api/trpc`), **Socket.io**, and databases still need a **separate long-running host** (e.g. Railway, Render, Fly.io, or a VPS) with `pnpm start` and your `.env`.

For a **split** setup (static site on Vercel + API elsewhere), point the browser at your backend:

1. Set **`VITE_API_BASE`** in Vercel to your API origin (e.g. `https://api.yourapp.com`) **at build time** if you wire the client to use it for tRPC (see `client/src/main.jsx` — today the tRPC URL is relative; you may need `${import.meta.env.VITE_API_BASE ?? ""}/api/trpc`).
2. Allow your **Vercel domain** in the backend **CORS** configuration.

If the Git repository nests this app in a subfolder, set **Root Directory** in the Vercel project to the folder that contains `package.json` and `vercel.json`.
