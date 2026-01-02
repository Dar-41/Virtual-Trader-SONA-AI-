# Deploy to Vercel - Trade Battle DS

## Architecture Overview
- **Frontend**: Next.js App (deployed on **Vercel**)
- **Backend**: Node.js/Socket.io Server (deployed on **Railway**)

> [!IMPORTANT]
> **Why separate deployments?**
> Vercel is serverless and cannot maintain the **persistent WebSocket connections** required for the real-time game. The backend MUST be deployed to a stateful host like Railway or Render.

---

## Step 1: Deploy Backend (Railway)
*If you haven't done this yet*

1.  Go to [Railway.app](https://railway.app).
2.  Deploy the repository.
3.  Set Root Directory to `server`.
4.  Copy the provided domain (e.g., `https://web-production-1234.up.railway.app`).

*(See `DEPLOY_NETLIFY.md` or `DEPLOY_RAILWAY.md` for detailed backend steps)*

---

## Step 2: Deploy Frontend (Vercel)

### Option A: Vercel Web Dashboard (Recommended)
1.  Go to [Vercel.com](https://vercel.com) and login.
2.  Click **"Add New..."** -> **"Project"**.
3.  Import the **`sona-ai-virtual-trader`** repository.
4.  **Configure Project**:
    - **Framework Preset**: Next.js (Auto-detected)
    - **Root Directory**: `./` (Default)
    - **Build Command**: `npm run build`
    - **Output Directory**: `.next`
5.  **Environment Variables**:
    - Add a new variable:
        - **Name**: `NEXT_PUBLIC_SOCKET_URL`
        - **Value**: (Your Railway Backend URL, e.g., `https://web-production-1234.up.railway.app`)
6.  Click **Deploy**.

### Option B: Vercel CLI
If you have the CLI installed:

```bash
# Login
vercel login

# Deploy production
vercel --prod
```

---

## Step 3: Link Backend to Frontend
Once Vercel deploys, you will get a URL (e.g., `https://sona-ai-virtual-trader.vercel.app`).

1.  Go back to your **Railway Project**.
2.  Go to **Variables**.
3.  Update/Add `FRONTEND_URL`:
    - Value: `https://sona-ai-virtual-trader.vercel.app` (Your Vercel URL)
4.  Railway will automatically redeploy the backend.

---

## Troubleshooting

### Socket Connection Failed
- Check browser console (F12).
- Verify `NEXT_PUBLIC_SOCKET_URL` in Vercel settings matches your Railway URL exactly (no trailing slash).
- Verify `FRONTEND_URL` in Railway matches your Vercel URL.
- Ensure Backend is actually running.

### 404 on API Routes
- This app uses a custom distinct backend. API routes should not be used on Vercel; all logic is via Sockets.
