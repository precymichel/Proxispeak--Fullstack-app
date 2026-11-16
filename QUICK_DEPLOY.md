# ProxiSpeak — Quick Public Deployment

This repo is configured for a quick portfolio demo using **Vercel (frontend) + Render (backend)**.

## 1. Push this project to GitHub

Upload the contents of this folder to a GitHub repository. Do not upload `.env` files,
`node_modules`, or real database/Redis credentials.

## 2. Deploy the backend on Render

Create a **New → Web Service** and connect this repository.

- Root Directory: `server`
- Runtime: `Node`
- Build Command: `npm ci`
- Start Command: `npm start`
- Instance: one instance for the first demo

Environment variables:
- `NODE_ENV` = `production`
- `CLIENT_ORIGIN` = `https://YOUR-VERCEL-PROJECT.vercel.app`
- `PORT` = `4000` (Render also supplies a port; the app reads `PORT`)
- `MONGODB_URI` = leave unset initially, or use a MongoDB Atlas connection string
- `REDIS_URL` = leave unset for a single backend instance

After deployment, test:
`https://YOUR-RENDER-SERVICE.onrender.com/api/health`

Expected JSON includes `"ok": true`. With MongoDB and Redis omitted, the response should
show `"persistence": false` and `"redisAdapter": false`; this is acceptable for a simple
single-instance demo. Presence and room events are held in that server process.

## 3. Deploy the frontend on Vercel

Create a Vercel project from the same repository.

- Root Directory: `client`
- Framework Preset: `Vite`
- Build Command: `npm run build`
- Output Directory: `dist`

Environment variable:
- `VITE_SERVER_URL` = `https://YOUR-RENDER-SERVICE.onrender.com`

Deploy/redeploy after setting the environment variable. Then open the Vercel URL.

## 4. Test the live app

1. Open the Vercel URL in two separate browser windows (or one normal and one incognito).
2. Enter different display names.
3. Use the same Workspace ID in both windows (for example `main-office`).
4. Check that both avatars appear and movement synchronizes.
5. Grant microphone permission and test voice while avatars are within 100 pixels.

## Important limitations to disclose in a portfolio

- The app currently uses Google's public STUN server only. WebRTC voice may fail on some
  corporate, mobile, or restrictive networks; reliable public voice generally needs a TURN
  relay and real cross-network testing.
- Render free instances may sleep when idle, so the first request after inactivity can be slow.
- A single Render instance is appropriate for a demo. Do not add multiple backend instances
  until Redis is configured and the shared presence behavior is tested.
- MongoDB is optional for this prototype. Add `MONGODB_URI` only after creating an Atlas
  database user and allowing the Render service to connect.
- This is a portfolio prototype, not a production-secure collaboration platform. The current
  README lists authentication, authorization, load testing, monitoring, and other production
  hardening still required.

## Local development

Server:
```bash
cd server
npm ci
npm run dev
```

Client in a second terminal:
```bash
cd client
npm ci
npm run dev
```

Create `server/.env` from `server/.env.example` and `client/.env` from
`client/.env.example` if you need to override defaults.
