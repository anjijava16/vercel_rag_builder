# Vercel Deployment Guide

This guide covers deploying your chat application's backend and frontend as separate Vercel projects.

## Prerequisites

1. Install Vercel CLI globally:
   ```bash
   npm install -g vercel
   ```

2. Login to Vercel:
   ```bash
   vercel login
   ```

## Part 1: Deploy Backend (FastAPI)

### Step 1: Navigate to Backend Directory
```bash
cd chat
```

### Step 2: Deploy to Vercel
```bash
vercel
```

When prompted:
- **Set up and deploy?** → Yes
- **Which scope?** → Select your account/team
- **Link to existing project?** → No
- **Project name?** → Choose a name (e.g., `chat-backend` or `rag-api`)
- **Directory?** → Press Enter (use current directory)
- **Override settings?** → No

### Step 3: Configure Environment Variables

After initial deployment, add environment variables via Vercel dashboard or CLI:

```bash
# Via CLI (from chat directory)
vercel env add BACKEND_API_KEY
# Enter your secret API key when prompted

vercel env add FRONTEND_URL
# Enter: https://your-frontend-url.vercel.app

# Add any other environment variables your RAG needs:
vercel env add OPENAI_API_KEY
vercel env add PINECONE_API_KEY
vercel env add GROQ_API_KEY
# etc.
```

Or add them via the Vercel Dashboard:
1. Go to your project → Settings → Environment Variables
2. Add each variable for Production, Preview, and Development

### Step 4: Deploy to Production
```bash
vercel --prod
```

**Save your backend URL** (e.g., `https://chat-backend-xxx.vercel.app`)

---

## Part 2: Deploy Frontend (Next.js)

### Step 1: Navigate to Frontend Directory
```bash
cd ../frontend/chat-frontend
```

### Step 2: Deploy to Vercel
```bash
vercel
```

When prompted:
- **Set up and deploy?** → Yes
- **Which scope?** → Select your account/team
- **Link to existing project?** → No
- **Project name?** → Choose a name (e.g., `chat-frontend`)
- **Directory?** → Press Enter (use current directory)
- **Override settings?** → No

### Step 3: Configure Environment Variables

Add your backend URL and API key:

```bash
# Via CLI (from frontend/chat-frontend directory)
vercel env add BACKEND_URL
# Enter your backend URL: https://chat-backend-xxx.vercel.app

vercel env add BACKEND_API_KEY
# Enter the SAME API key you set for the backend
```

Or via Vercel Dashboard:
1. Go to your project → Settings → Environment Variables
2. Add:
   - `BACKEND_URL`: `https://your-backend-url.vercel.app`
   - `BACKEND_API_KEY`: Your secret API key (same as backend)

### Step 4: Deploy to Production
```bash
vercel --prod
```

**Save your frontend URL** (e.g., `https://chat-frontend-xxx.vercel.app`)

---

## Part 3: Update CORS Configuration

### Update Backend CORS

Now that you have your frontend URL, update the backend's `FRONTEND_URL` environment variable:

```bash
cd ../../chat
vercel env add FRONTEND_URL production
# Enter your actual frontend URL
```

Or update via Vercel Dashboard → Backend Project → Settings → Environment Variables

### Redeploy Backend
```bash
vercel --prod
```

---

## Verification

### Test Backend Health
```bash
curl https://your-backend-url.vercel.app/health
```

Expected response:
```json
{"status": "healthy", "message": "Order Documents RAG API is running"}
```

### Test Frontend
Open your frontend URL in a browser and try asking a question in the chat interface.

---

## Continuous Deployment

### Option 1: Git Integration (Recommended)

1. Push your code to GitHub/GitLab/Bitbucket
2. In Vercel Dashboard:
   - Import your repository
   - Configure root directory for each project:
     - Backend: `chat`
     - Frontend: `frontend/chat-frontend`
3. Every push to main/master will auto-deploy

### Option 2: Manual Deployment

From the respective directories, run:
```bash
vercel --prod
```

---

## Environment Variables Summary

### Backend (`chat`)
- `BACKEND_API_KEY` - Secret key for API authentication
- `FRONTEND_URL` - Your frontend URL for CORS
- Plus any AI service keys (OpenAI, Pinecone, Groq, etc.)

### Frontend (`frontend/chat-frontend`)
- `BACKEND_URL` - Your backend API URL
- `BACKEND_API_KEY` - Same secret key as backend

---

## Troubleshooting

### CORS Errors
- Ensure `FRONTEND_URL` in backend matches your actual frontend URL
- Include the protocol (`https://`) in the URL
- No trailing slash

### API Key Errors
- Ensure `BACKEND_API_KEY` is the same in both projects
- Check it's set for the correct environment (production/preview/development)

### Backend Not Found
- Verify `BACKEND_URL` in frontend includes protocol (`https://`)
- No trailing slash in the URL
- Check backend health endpoint first

### Environment Variables Not Working
- After adding env vars, redeploy the project
- Environment variables are only available at build time for Next.js (unless using `NEXT_PUBLIC_` prefix)
- For server-side env vars, they're available in API routes and server components

---

## Monitoring

- View logs in Vercel Dashboard → Your Project → Deployments → Select deployment → Runtime Logs
- Set up monitoring and alerts in Vercel Dashboard → Your Project → Settings → Monitoring

---

## Cost Optimization

- **Hobby Plan**: Free for personal projects, includes both frontend and backend
- **Pro Plan**: Required for commercial use, better performance and support
- Monitor usage in Vercel Dashboard → Settings → Usage

---

## Next Steps

1. Set up custom domain (optional)
2. Configure CI/CD via Git integration
3. Set up monitoring and error tracking
4. Add rate limiting rules if needed
5. Enable Vercel Analytics on frontend
