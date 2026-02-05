# Route Optimizer Deployment Guide

This app can be deployed to multiple platforms. Python version is specified in `.python-version` file (compatible with Render, Northflank, Railway, and most modern platforms).

## Deploy to Render (Free)

### Step 1: Prepare Your Code

1. **Update API Key in Production**
   - Open `templates/index.html`
   - Replace the API key with your production key
   - Add API key restrictions in Google Cloud Console:
     - HTTP referrer: `https://your-app-name.onrender.com/*`

2. **Commit Files**
   ```bash
   git init
   git add .
   git commit -m "Initial commit for route optimizer"
   ```

3. **Push to GitHub**
   - Create a new repository on GitHub
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/route-optimizer.git
   git branch -M main
   git push -u origin main
   ```

### Step 2: Deploy on Render

1. **Create Render Account**
   - Go to https://render.com
   - Sign up (GitHub login recommended)

2. **Create New Web Service**
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Select the branch (main)

3. **Configure Settings**
   
   **Build & Deploy**:
   - Runtime: `Python 3`
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `python app.py`

   **Advanced**:
   - Instance Type: `Free`
   - Region: Choose closest to your users

4. **Set Environment Variables** (Optional but recommended)
   - Add `FLASK_ENV=production`
   - Add `FLASK_DEBUG=0`

5. **Deploy**
   - Click "Create Web Service"
   - Wait 2-5 minutes for deployment
   - Your app will be available at: `https://your-app-name.onrender.com`

### Step 3: Test Your App

1. Visit your Render URL
2. Check if the map loads correctly
3. Test adding waypoints and optimizing routes

### Common Issues & Solutions

#### App Shows "Google Maps Error"
- **Cause**: API key restrictions
- **Fix**: Add your Render URL to API key HTTP referrers in Google Cloud Console

#### App Loads Slowly
- **Cause**: Render free tier spins down when idle
- **Fix**: First load takes 30-60 seconds, subsequent loads are faster

#### API Key Security Warning
- **Fix**: Never commit API keys to public repos
- **Better**: Use environment variables and Flask config

### Updating Your App

1. Make changes locally
2. Commit and push to GitHub
3. Render automatically redeploys on push

### Monitoring

- View logs in Render Dashboard → Your Service → Logs
- Monitor usage in Dashboard → Metrics

---

## Alternative: Deploy to Northflank

1. Go to https://northflank.com
2. Create an account and new project
3. Click "Add Service" → "Combined Service"
4. Connect your GitHub repository
5. Configure:
   - **Build Type**: Buildpack
   - **Python Version**: Auto-detected from `.python-version` (3.9.19)
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `python app.py`
6. Add environment variables:
   ```
   PORT=8080
   FLASK_ENV=production
   ```
7. Deploy

**Note**: Northflank uses `.python-version` file (not `runtime.txt`) to detect Python version.

---

## Alternative: Deploy to Railway

1. Go to https://railway.app
2. Click "Deploy New Project"
3. Select "Deploy from GitHub repo"
4. Choose your repository
5. Click "Add Variables" (optional)
6. Click "Deploy"

---

## Alternative: Deploy to PythonAnywhere

1. Go to https://pythonanywhere.com
2. Sign up and create an account
3. Click "Web" → "Add a new web app"
4. Select "Flask" and Python version
5. Upload files or use git clone
6. Configure the app

---

## Free Domain Options

Your free Render subdomain: `https://your-route-optimizer.onrender.com`

To get a custom domain:
- **Paid**: Buy from Namecheap, GoDaddy, etc. (~$10-15/year)
- Configure DNS to point to your Render app
- Add domain in Render Dashboard → Domains

---

## Next Steps After Deployment

1. **Monitor API Usage**
   - Check Google Cloud Console for API quotas
   - Free tier limits: 100k Distance Matrix elements/day

2. **Add Analytics**
   - Consider Google Analytics for usage tracking

3. **Improve Performance**
   - Consider caching geocoded addresses
   - Add loading indicators

4. **Backup**
   - Keep a copy of your code
   - Save configuration settings

---

## Cost Summary

| Service | Cost | Domain |
|----------|------|--------|
| Render (Free Tier) | $0/month | `.onrender.com` |
| Google Maps API | Free tier (100k elements/day) | - |
| Custom Domain | ~$10-15/year | Your choice |

**Total**: Free with Render subdomain, or ~$10-15/year with custom domain
