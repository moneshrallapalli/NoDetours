# Quick Render.com Deployment Guide - Step by Step

**Total Time: ~20 minutes**

---

## STEP 1: Get API Keys (5 minutes)

You need at least **ONE** LLM API key. Choose one:

### Option A: OpenAI (GPT-3.5 / GPT-4)
1. Go to: https://platform.openai.com/api-keys
2. Sign in (create account if needed)
3. Click "Create new secret key"
4. Copy the key (starts with `sk-`)
5. **Save it somewhere safe** ✓

### Option B: Anthropic Claude (RECOMMENDED - Cheaper & Better)
1. Go to: https://console.anthropic.com/
2. Sign in (create account if needed)
3. Go to "API Keys" section
4. Click "Create Key"
5. Copy the key (starts with `sk-ant-`)
6. **Save it somewhere safe** ✓

**Optional but Recommended** (for better experience):
- Weather API: https://www.weatherapi.com/ (free tier)
- Google Maps: https://console.cloud.google.com/ (free $200 credit)
- Google Search: https://cse.google.com/cse/ (free tier)

---

## STEP 2: Commit Files to GitHub (2 minutes)

Open terminal in your NoDetours directory:

```bash
cd /Users/monesh/University/NoDetours

# Check if all files are ready
git status

# You should see new files like:
# render.yaml, Procfile, RENDER_SETUP.md, etc.

# Add all files
git add .

# Commit
git commit -m "chore: add production deployment configuration"

# Push to GitHub
git push origin main
```

**Done?** ✓ Continue to Step 3

---

## STEP 3: Create Render.com Account (2 minutes)

1. Go to: **https://render.com**
2. Click **"Sign up"** (top right)
3. Choose **"GitHub"** option (easiest!)
4. Click **"Authorize render-ci"** (allows Render to access your repos)
5. You're now logged in ✓

---

## STEP 4: Create Web Service on Render (5 minutes)

1. In your Render dashboard, click **"New +"** (top right)
2. Select **"Web Service"**
3. Find and select **"NoDetours"** repository
4. Fill in the form with these values:

| Field | Value |
|-------|-------|
| Name | `nodetours` |
| Region | `oregon` (or closest to you) |
| Branch | `main` |
| Runtime | `Python 3` |
| Build Command | `pip install -r requirements.txt` |
| Start Command | `gunicorn -w 4 -b 0.0.0.0:$PORT --timeout 120 api.app:app` |
| Instance Type | `Starter` ($7/month) |

5. Click **"Create Web Service"** button
6. **Wait** for build to complete (you'll see "Building..." status)
   - This takes 2-3 minutes ⏳

**You'll see**: Build logs → "Build complete" → Service deployed ✓

---

## STEP 5: Add Environment Variables (5 minutes)

Once deployment is done:

1. In your service dashboard, click **"Environment"** tab (top menu)
2. You'll see "Add Environment Variable" button

### Add your LLM API Key:

**For OpenAI:**
```
Name:  OPENAI_API_KEY
Value: sk-... (paste your OpenAI key)
```
Click "Save"

**OR For Anthropic (Recommended):**
```
Name:  ANTHROPIC_API_KEY
Value: sk-ant-... (paste your Anthropic key)
```
Click "Save"

### Add Optional API Keys (recommended):

```
Name:  WEATHER_API_KEY
Value: (your weather api key)
```
Click "Save"

```
Name:  MAPS_API_KEY
Value: (your Google Maps API key)
```
Click "Save"

```
Name:  SEARCH_API_KEY
Value: (your Google Search API key)
```
Click "Save"

```
Name:  SEARCH_ENGINE_ID
Value: (your Google Search Engine ID)
```
Click "Save"

### Add Application Configuration:

```
Name:  LOG_LEVEL
Value: INFO
```
Click "Save"

```
Name:  APP_ENV
Value: production
```
Click "Save"

**After adding variables**: Service auto-restarts (watch the logs)

---

## STEP 6: Verify Deployment Works (3 minutes)

1. In your service dashboard, look for your **Service URL**
   - It looks like: `https://nodetours.onrender.com`
   - Click on it to open your app ✓

2. Test with a travel query:
   ```
   "Help me plan a 3-day trip to Paris with museums and local food"
   ```

3. Click **"Create Travel Plan"** button

4. **Wait** ~10 seconds (first request is slower)

5. You should see:
   - ✅ Itinerary (day-by-day schedule)
   - ✅ Packing List (what to bring)
   - ✅ Budget Estimate (cost breakdown)
   - ✅ Download Calendar button

**Success?** ✓ Skip to "What's Next"

**Not working?**
- Check the "Logs" tab in Render for error messages
- Verify API keys are correct (no extra spaces)
- See "Troubleshooting" section below

---

## STEP 7: Update Your Profile (2 minutes)

### Update README.md

Open your `README.md` file and add at the top (after the title):

```markdown
## Live Demo

Try NoDetours live: **[https://nodetours.onrender.com](https://nodetours.onrender.com)**

Example query: "Help me plan a 7-day trip to Japan focusing on anime sites and local food"
```

### Update GitHub Profile Bio

1. Go to: https://github.com/your-username
2. Click "Edit profile"
3. Update bio to something like:

```
🚀 AI Systems Engineer | LLM Integration Specialist
Building production-grade AI applications with thoughtful system design
Live demo: https://nodetours.onrender.com
```

4. Save changes ✓

---

## WHAT YOU'LL SEE

### During Build (2-3 minutes)
```
Building Docker image...
Fetching requirements.txt...
Installing packages...
✓ Build complete
```

### Service Running
```
✓ Service is live at https://nodetours.onrender.com
Ready to receive requests
```

### First Request Performance
- **First request**: ~10-15 seconds (normal, cold start)
- **After that**: ~6-7 seconds (normal speed)

---

## TROUBLESHOOTING

### Problem: Build Failed
**Check**: Look at the "Logs" tab for error message

**Common causes**:
- Missing dependency → Update requirements.txt
- Python version issue → Check Procfile matches Render runtime

**Solution**: Fix locally, push to GitHub, redeploy

### Problem: Service Crashes
**Check**: Click "Logs" tab to see error

**Common causes**:
- Missing environment variable → Add it in "Environment" tab
- API key not set → Verify key is in Environment tab
- API key is invalid → Get a new one and update

**Solution**: Fix environment variable, service auto-restarts

### Problem: API Calls Failing
**This is OK!** App still works but without that data

**Examples**:
- No weather data → Shows itinerary without weather context
- No maps data → Still shows destination info
- No search results → Uses general knowledge

**Solution**: Optional APIs failing won't break anything

### Problem: "Service URL not responding"
**Wait**: Takes 2-3 minutes after deployment

**Try**: Refresh page after waiting

**Check**: Are you at the correct URL? (https://nodetours.onrender.com)

---

## COST BREAKDOWN

### Render.com Hosting
- **Starter Plan**: $7/month ✓ (recommended for this project)
- **Standard Plan**: $12/month (if you want better performance)
- **Free Tier**: Available but services spin down after inactivity

### API Costs (Monthly, 100 requests/day)
- **Anthropic Claude**: ~$78/month ✓ (best value)
- **OpenAI GPT-4**: ~$390/month (expensive)
- **OpenAI GPT-3.5**: ~$9/month (cheapest but lower quality)
- **Weather API**: Free tier available
- **Google APIs**: Free tier available

**Total Monthly Cost**: ~$85 (Render + Anthropic Claude)

---

## MONITORING

### Check Performance
In Render dashboard:
- **Logs** tab: See what's happening
- **Metrics** tab: CPU, memory, requests
- **Deployments** tab: Deployment history

### Typical Metrics
- CPU: 0-30% (at rest)
- Memory: 150-300 MB
- Response time: 6-7 seconds
- Uptime: 99%+ (very stable)

---

## WHAT'S NEXT

### Immediate (Today)
- [ ] Deploy to Render (follow steps above)
- [ ] Test with sample query
- [ ] Verify everything works

### This Week
- [ ] Update README with live URL
- [ ] Update GitHub profile bio
- [ ] Share URL with recruiters
- [ ] Monitor performance

### Next Week
- [ ] Share in job applications
- [ ] Use in technical interviews
- [ ] Get feedback from people who test it
- [ ] Optimize based on feedback

---

## QUICK REFERENCE

**Your Live URL**: `https://nodetours.onrender.com`

**Dashboard**: `https://render.com/dashboard`

**Logs**: Click "Logs" in service dashboard

**Environment Variables**: Click "Environment" in service dashboard

**Restart Service**: Click three dots → "Restart service"

**View Metrics**: Click "Metrics" tab

---

## NEED HELP?

**Deployment issues**: See full DEPLOYMENT.md for detailed troubleshooting

**Architecture questions**: See TECHNICAL_DECISIONS.md

**Performance questions**: See BENCHMARKS.md

**General help**: Check IMPLEMENTATION_COMPLETE.md

---

## SUCCESS CHECKLIST

- ✅ API keys obtained
- ✅ Files committed to GitHub
- ✅ Render account created
- ✅ Web Service created
- ✅ Build completed successfully
- ✅ Environment variables added
- ✅ Service restarted
- ✅ Live URL works
- ✅ Sample query tested
- ✅ README updated with live URL
- ✅ GitHub profile bio updated

**If all checked**: You're done! 🎉 Share your live URL with the world!

---

## TIME BREAKDOWN

| Step | Time | Cumulative |
|------|------|-----------|
| Get API Keys | 5 min | 5 min |
| Commit to GitHub | 2 min | 7 min |
| Create Render account | 2 min | 9 min |
| Create Web Service | 5 min | 14 min |
| Add Environment Variables | 5 min | 19 min |
| Verify & Test | 3 min | 22 min |
| Update Profile | 2 min | 24 min |
| **TOTAL** | **~25 min** | **Live!** |

---

**You're ready! Follow the steps above and you'll be live in under 30 minutes.** 🚀
