# 🔧 Fix Vercel 404 NOT_FOUND Error

## The Problem
Vercel deployment is failing with 404 NOT_FOUND. This is usually caused by:
1. Missing environment variables (most common)
2. Database migration not running
3. Build configuration issues

## ✅ SOLUTION - Follow These Steps:

### Step 1: Check Environment Variables

Go to your Vercel project → **Settings** → **Environment Variables**

**Make sure ALL of these are added:**

```
DATABASE_URL          = [Auto-added by Vercel Postgres]
NEXTAUTH_SECRET       = [Generate: any 32+ random characters]
NEXTAUTH_URL          = https://your-project.vercel.app
APP_URL               = https://your-project.vercel.app
RESEND_API_KEY        = re_your_api_key_here
FROM_EMAIL            = noreply@yourdomain.com
STRAVA_CLUB_ID        = 1944957
STRAVA_CLUB_URL       = https://www.strava.com/clubs/1944957
ENABLE_CRON_JOBS      = true
CRON_SECRET           = [Generate: any 32+ random characters]
```

**CRITICAL:** Make sure to select **Production**, **Preview**, AND **Development** for each variable!

### Step 2: Add Vercel Postgres Database (If Not Done)

1. Go to **Storage** tab in your Vercel project
2. Click **Create Database**
3. Select **Postgres**
4. Name: `rossi-tracker-db`
5. Click **Create**

This automatically adds `DATABASE_URL` to your environment variables.

### Step 3: Update Build Settings

1. Go to **Settings** → **General**
2. Scroll to **Build & Development Settings**
3. **Framework Preset**: Next.js ✅
4. **Build Command**: 
   ```
   npm run build
   ```
5. **Install Command**: 
   ```
   npm install && npx prisma generate
   ```

### Step 4: Add Database Migration Script

We need to run migrations after install. Update your `package.json`:

In the GitHub repository, edit `package.json` and change the `postinstall` script:

**BEFORE:**
```json
"postinstall": "prisma generate"
```

**AFTER:**
```json
"postinstall": "prisma generate && prisma migrate deploy"
```

Commit and push this change to GitHub.

### Step 5: Redeploy

1. Go to **Deployments** tab
2. Click the three dots (...) on the latest deployment
3. Click **Redeploy**
4. ✅ Check "Use existing Build Cache"
5. Click **Redeploy**

---

## 🚨 If Still Getting 404 Error:

### Check the Build Logs

1. Click on the failed deployment
2. Look at the **Build Logs**
3. Common errors:

#### Error: "Could not find a production build"
**Fix:** Environment variables missing
- Add all variables listed in Step 1
- Redeploy

#### Error: "Prisma schema not found"
**Fix:** 
```bash
# In your GitHub repo, make sure these files exist:
prisma/schema.prisma
src/lib/prisma.ts
```

#### Error: "Can't reach database server"
**Fix:** 
- Go to Storage tab
- Make sure Postgres database is created
- Check DATABASE_URL is in environment variables

#### Error: "Module not found"
**Fix:**
```bash
# Make sure package.json has all dependencies
# Should include:
- next
- react
- @prisma/client
- next-auth
```

---

## 🎯 Quick Fix Checklist

Run through this checklist:

- [ ] Vercel Postgres database created
- [ ] DATABASE_URL environment variable exists
- [ ] NEXTAUTH_SECRET added (any random 32 chars)
- [ ] NEXTAUTH_URL = your Vercel app URL
- [ ] APP_URL = your Vercel app URL
- [ ] RESEND_API_KEY added (from resend.com)
- [ ] FROM_EMAIL added
- [ ] All environment variables set for Production, Preview, Development
- [ ] Build command is `npm run build`
- [ ] Framework preset is Next.js
- [ ] Redeployed after adding variables

---

## 🔍 Alternative: Check What's Missing

### Method 1: View Deployment Logs

1. Click on failed deployment
2. Click **View Build Logs**
3. Scroll to the error
4. Share the error message with me for specific help

### Method 2: Use Vercel CLI (Advanced)

```bash
# Install Vercel CLI
npm i -g vercel

# Link to project
vercel link

# Pull environment variables
vercel env pull

# Deploy
vercel --prod
```

---

## 💡 Most Common Solutions:

### Solution A: Missing DATABASE_URL
1. Create Vercel Postgres (Storage tab)
2. DATABASE_URL auto-added
3. Redeploy

### Solution B: Missing NEXTAUTH_SECRET
1. Generate random string: https://generate-secret.vercel.app/32
2. Add as environment variable
3. Redeploy

### Solution C: Wrong NEXTAUTH_URL
1. Copy your Vercel app URL
2. Update NEXTAUTH_URL to match exactly
3. Update APP_URL to match exactly
4. Redeploy

---

## 🆘 Still Stuck?

Share with me:
1. Screenshot of environment variables (blur sensitive values)
2. Build log error message
3. Whether you created Vercel Postgres database

I'll help you debug further!

---

## ✨ Once Fixed, You Should See:

✅ Build succeeds  
✅ Deployment successful  
✅ App loads at your-project.vercel.app  
✅ You can register an account  
✅ Email verification works  

Then you're live! 🎉
