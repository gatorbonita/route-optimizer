# Northflank CI/CD Setup Guide

This guide explains how to enable automatic deployments from GitHub to Northflank.

## Option 1: Enable Git Integration (Easiest) ⭐

**This is the recommended approach - no GitHub Actions needed!**

1. **Go to Northflank Dashboard**
   - Navigate to your project
   - Click on your service

2. **Connect to GitHub**
   - Go to **"Git"** or **"Source"** tab
   - Click **"Connect Repository"**
   - Authorize Northflank to access your GitHub account
   - Select repository: `gatorbonita/route-optimizer`
   - Select branch: `main`

3. **Enable Auto-Deploy**
   - Toggle **"Auto-deploy on push"** or **"Build on commit"**
   - Save settings

4. **Test It**
   - Push a commit to `main` branch
   - Northflank should automatically start building
   - Check the **"Builds"** tab to see progress

✅ **Done!** Now every push to `main` will automatically deploy.

---

## Option 2: Use GitHub Actions with Webhook

If Option 1 doesn't work, use GitHub Actions to trigger deployments:

### Step 1: Get Northflank Webhook URL

1. Go to Northflank Dashboard → Your Service
2. Click **"Settings"** → **"Webhooks"** or **"Build Settings"**
3. Find or create a **"Build Webhook"** or **"Deploy Webhook"**
4. Copy the webhook URL (looks like: `https://api.northflank.com/v1/webhooks/...`)

### Step 2: Add Secret to GitHub

1. Go to your GitHub repository
2. Click **"Settings"** → **"Secrets and variables"** → **"Actions"**
3. Click **"New repository secret"**
4. Name: `NORTHFLANK_WEBHOOK_URL`
5. Value: Paste the webhook URL from Step 1
6. Click **"Add secret"**

### Step 3: GitHub Actions Already Configured

The workflow file `.github/workflows/deploy-northflank.yml` is already in the repo.

It will:
- ✅ Trigger on every push to `main` branch
- ✅ Call Northflank webhook to start deployment
- ✅ Can be manually triggered from GitHub Actions tab

### Step 4: Test It

1. Push a commit to `main` branch
2. Go to GitHub → Actions tab
3. You should see the workflow running
4. Check Northflank dashboard for deployment

---

## Option 3: Manual Deployment

If you prefer manual control:

1. **Via Northflank Dashboard**
   - Go to your service
   - Click **"Redeploy"** or **"Build & Deploy"**
   - Select latest commit
   - Deploy

2. **Via Northflank CLI**
   ```bash
   # Install Northflank CLI
   npm install -g @northflank/cli

   # Login
   northflank login

   # Deploy
   northflank deploy --project PROJECT_ID --service SERVICE_ID
   ```

---

## Troubleshooting

### GitHub Actions not triggering?
- Check if `.github/workflows/deploy-northflank.yml` is in `main` branch
- Verify the secret `NORTHFLANK_WEBHOOK_URL` is set correctly
- Check GitHub Actions tab for error messages

### Northflank not building?
- Verify webhook URL is correct
- Check Northflank logs for errors
- Ensure Dockerfile builds successfully locally first

### Build succeeds but old version still deployed?
- Check if port 8080 is correct in Northflank settings
- Verify environment variables are set
- Try a manual redeploy from Northflank dashboard

---

## Recommended Setup

For the best experience:

1. **Use Option 1** (Git Integration) if available in your Northflank plan
2. **Use Option 2** (GitHub Actions) if Git Integration isn't available
3. Keep Docker builds fast by using `.dockerignore` properly
4. Monitor deployments in both GitHub Actions and Northflank dashboard

---

## Current Status

✅ GitHub Actions workflow configured: `.github/workflows/deploy-northflank.yml`
✅ Dockerfile optimized for Northflank deployment
✅ Ready to enable CI/CD using either option above

Choose the option that works best for your setup!
