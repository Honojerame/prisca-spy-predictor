# Deployment Guide for PRISCA

## Quick Deploy Options

### Option 1: Render (Recommended - Easiest)

**Why Render?**
- Free tier available
- Automatic deployments from GitHub
- Supports both frontend and backend
- Built-in SSL certificates

**Steps:**

1. **Push to GitHub** (already done ✓)

2. **Deploy Backend:**
   - Go to https://render.com and sign up/login
   - Click "New +" → "Web Service"
   - Connect your GitHub repository: `Honojerame/prisca-spy-predictor`
   - Configure:
     - **Name**: `prisca-backend`
     - **Root Directory**: Leave empty
     - **Environment**: `Python 3`
     - **Build Command**: `pip install -r backend/requirements.txt`
     - **Start Command**: `cd backend && python -m uvicorn app:app --host 0.0.0.0 --port $PORT`
   - Click "Create Web Service"
   - Wait 5-10 minutes for deployment
   - Copy the deployed URL (e.g., `https://prisca-backend.onrender.com`)

3. **Deploy Frontend:**
   - Click "New +" → "Static Site"
   - Connect same repository
   - Configure:
     - **Name**: `prisca-frontend`
     - **Root Directory**: Leave empty
     - **Build Command**: `echo "Static site"`
     - **Publish Directory**: `frontend`
   - Click "Create Static Site"
   
4. **Update Frontend API URL:**
   - Edit `frontend/index.html` line 549
   - Replace `'https://prisca-backend.onrender.com'` with your actual backend URL
   - Commit and push changes
   - Render will auto-deploy

5. **Done!** Your site will be live at `https://prisca-frontend.onrender.com`

**⚠️ Important Notes:**
- Free tier sleeps after 15 minutes of inactivity (first request takes ~30 seconds)
- For production, upgrade to paid plan ($7/month for always-on service)

---

### Option 2: Vercel (Frontend) + Render (Backend)

**Why this combo?**
- Vercel has better frontend performance
- Free and fast
- Good for showcasing

**Steps:**

1. **Deploy Backend on Render** (same as above steps 2)

2. **Deploy Frontend on Vercel:**
   - Go to https://vercel.com and sign up/login
   - Click "Add New..." → "Project"
   - Import your GitHub repository
   - Configure:
     - **Framework Preset**: Other
     - **Root Directory**: `frontend`
     - **Build Command**: Leave empty
     - **Output Directory**: Leave empty
   - Update API URL in `frontend/index.html` with your Render backend URL
   - Click "Deploy"

3. **Done!** Your site will be at `https://prisca.vercel.app`

---

### Option 3: GitHub Pages (Frontend Only)

**Limitations:** Backend won't be deployed, so predictions won't work unless you deploy backend separately.

**Steps:**
1. Go to your repo settings → Pages
2. Source: Deploy from a branch → `main` → `/frontend`
3. Save and wait
4. Site will be at `https://shaaronl.github.io/market-mood-to-financial-moves/`

---

## Post-Deployment Checklist

- [ ] Backend health check works: `https://your-backend.onrender.com/health`
- [ ] Frontend loads properly
- [ ] Prediction button works
- [ ] Charts display correctly
- [ ] No CORS errors in browser console

---

## Cost Summary

| Platform | Free Tier | Limitations |
|----------|-----------|-------------|
| Render | Yes | Sleeps after 15 min inactivity |
| Vercel | Yes | 100GB bandwidth/month |
| Railway | $5 credit/month | After credit runs out, needs payment |

---

## Recommended Setup for Presentation

**Best:** Render (both frontend + backend)
- Single platform
- Easy to manage
- Professional URLs
- Free tier sufficient

**Share with team:**
- Frontend URL: `https://prisca-frontend.onrender.com`
- API docs: `https://prisca-backend.onrender.com/docs`

---

## Troubleshooting

**Issue:** Prediction doesn't work
- Check browser console (F12) for errors
- Verify backend URL is correct in `frontend/index.html`
- Check CORS settings in `backend/app.py`

**Issue:** Backend is slow on first load
- This is normal on free tier (cold start)
- Consider upgrading to paid tier if presenting live

**Issue:** CORS error
- Backend already has `allow_origins=["*"]` configured
- If deploying to production, update to specific domain

---

## Want to Deploy Now?

Run these commands:
```bash
# Already done - code is on GitHub ✓

# Next: Go to render.com and follow steps above
```
