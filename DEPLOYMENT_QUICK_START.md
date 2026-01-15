# Quick Start: Deploy to Vercel

## TL;DR

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy Backend
cd chat
vercel
vercel env add BACKEND_API_KEY
vercel env add FRONTEND_URL
vercel --prod

# Deploy Frontend
cd ../frontend/chat-frontend
vercel
vercel env add BACKEND_URL  # Use backend URL from previous step
vercel env add BACKEND_API_KEY  # Same key as backend
vercel --prod
```

## What You Need

### Backend Environment Variables
- `BACKEND_API_KEY` - Your secret API key (create a strong random string)
- `FRONTEND_URL` - Your frontend URL (get this after deploying frontend)
- Any AI service keys (OpenAI, Pinecone, Groq, etc.)

### Frontend Environment Variables
- `BACKEND_URL` - Your backend URL (get this after deploying backend)
- `BACKEND_API_KEY` - Same key as backend

## Deployment Order

1. **Deploy Backend First** → Get backend URL
2. **Deploy Frontend** → Use backend URL in env vars
3. **Update Backend** → Add frontend URL to CORS
4. **Redeploy Backend** → Apply CORS settings

## Testing

Backend:
```bash
curl https://your-backend-url.vercel.app/
```

Frontend: Open `https://your-frontend-url.vercel.app` in browser

---

**For detailed instructions, see [VERCEL_DEPLOYMENT_GUIDE.md](./VERCEL_DEPLOYMENT_GUIDE.md)**
