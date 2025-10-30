# Render.com Deployment Setup - Quick Reference

**Time Required**: 15-20 minutes
**Difficulty**: Beginner-Friendly
**Cost**: $12/month (Starter) or $7/month (Free tier)

---

## Pre-Deployment Checklist (Do This First)

### 1. Get Your API Keys (5 minutes)

**Must Have (At least one):**
- [ ] OpenAI API Key: https://platform.openai.com/api-keys
- [ ] OR Anthropic Claude Key: https://console.anthropic.com/

**Recommended (Highly):**
- [ ] Weather API Key: https://www.weatherapi.com/ (free tier)
- [ ] Google Maps API Key: https://console.cloud.google.com/
- [ ] Google Search API Key + Engine ID: https://cse.google.com/cse/

**Optional (But Nice):**
- [ ] FireCrawl API Key: https://www.firecrawl.dev/

### 2. Ensure Files are Committed to GitHub

```bash
# In your NoDetours directory
git status

# You should see these new files:
# - render.yaml
# - Procfile
# - .env.example
# - requirements.txt (updated)
# - .gitignore
# - DEPLOYMENT.md
# - TECHNICAL_DECISIONS.md
# - BENCHMARKS.md
# - CONTRIBUTING.md
# - ABOUT.md

# If not present, add them:
git add .
git commit -m "chore: add production deployment configuration and documentation"
git push origin main
```

---

## Step-by-Step Render.com Deployment

### Step 1: Sign Up (2 minutes)

1. Go to **https://render.com**
2. Click **"Sign up"**
3. Choose **"GitHub"** (easiest integration)
4. Authorize Render to access your GitHub account
5. Select repositories to allow (or allow all)

### Step 2: Create Web Service (5 minutes)

1. In Render dashboard, click **"New +"** → **"Web Service"**
2. Search for and select **"NoDetours"** repository
3. Fill in the form:

| Field | Value | Notes |
|-------|-------|-------|
| Name | `nodetours` | Becomes part of URL |
| Region | `oregon` | Choose closest region |
| Branch | `main` | Default main branch |
| Runtime | `Python 3` | Automatically selected |
| Build Command | `pip install -r requirements.txt` | Pre-filled if render.yaml exists |
| Start Command | `gunicorn -w 4 -b 0.0.0.0:$PORT --timeout 120 api.app:app` | Pre-filled if render.yaml exists |
| Instance Type | `Starter` | Sufficient for demo/testing |

4. Scroll to bottom, click **"Create Web Service"**
5. **Wait** for build to complete (2-3 minutes)

### Step 3: Add Environment Variables (5 minutes)

Once service is created:

1. In service dashboard, click **"Environment"** tab
2. Click **"Add Environment Variable"** for each key:

**LLM Providers (choose at least one):**
```
Name: OPENAI_API_KEY
Value: sk-... (your actual key)
```

OR

```
Name: ANTHROPIC_API_KEY
Value: sk-ant-... (your actual key)
```

**External APIs (recommended):**
```
Name: WEATHER_API_KEY
Value: (your actual key)

Name: MAPS_API_KEY
Value: (your actual key)

Name: SEARCH_API_KEY
Value: (your actual key)

Name: SEARCH_ENGINE_ID
Value: (your actual ID)

Name: FIRECRAWL_API_KEY
Value: (your actual key)
```

**Application Configuration:**
```
Name: LOG_LEVEL
Value: INFO

Name: APP_ENV
Value: production
```

3. After adding all variables, scroll down and click **"Save"**
4. Service will auto-restart with new variables (~30 seconds)

### Step 4: Verify Deployment (3 minutes)

1. In service dashboard, look for your **service URL** (e.g., `https://nodetours.onrender.com`)
2. Click the URL to open your application
3. Test with a sample query:
   - Enter: `"Help me plan a 3-day trip to Paris"`
   - Click "Create Travel Plan"
   - Wait for response (first request is slower, ~10 seconds)
   - Verify itinerary, packing list, and budget appear

**If it works**: ✅ You're deployed! Skip to "What's Next"

**If you see errors**:
- Click "Logs" tab to see what went wrong
- Check that all API keys are entered correctly
- Verify no extra spaces in variable values
- See "Troubleshooting" section below

---

## What You'll See During Deployment

### Build Log (appears after 30 seconds)
```
Building Docker image...
Fetching requirements.txt...
Installing Python packages...
Build complete!
```

### Service Running
```
Uvicorn running on 0.0.0.0:8000
Application started successfully
Listening for requests...
```

### When You Access the URL
- **First load** (1st request): ~10 seconds (cold start)
- **Subsequent requests**: ~6-7 seconds (normal response time)

---

## Troubleshooting

### "Build Failed"
**Check**: Do all dependencies install locally?
```bash
pip install -r requirements.txt
```

If that fails, update requirements.txt and push to GitHub.

### "Service Crashes on Startup"
**Check logs**: Look in "Logs" tab of Render dashboard
```
ModuleNotFoundError: No module named 'fastapi'
→ Missing dependency in requirements.txt

AttributeError: 'NoneType' object...
→ Missing environment variable
```

**Solution**:
1. Check error message in logs
2. Fix issue locally
3. Push to GitHub
4. Service auto-redeploys

### "Timeout Error" / "Application Not Responding"
**This is normal for first request** (cold start)
- First request: ~10-15 seconds (acceptable)
- Subsequent requests: ~6-7 seconds (normal)
- If consistently slow: Check logs for errors

### "API key errors"
**Check**:
- Key is entered exactly (no extra spaces)
- Key is not truncated
- Key is for the right provider (OpenAI vs Anthropic)
- Key is still valid (not revoked)

**Solution**: In Render dashboard Environment tab, re-enter the key

### "Missing API Response"
If weather, maps, or search doesn't work:
- Application still generates itinerary (graceful degradation)
- Check if you have API keys for those services
- If missing, just add the key and save

---

## Your Live Application URL

After successful deployment, you'll have a URL like:

```
https://nodetours.onrender.com
```

### Share It!
- Update README.md with link: `[Live Demo](https://nodetours.onrender.com)`
- Add to GitHub profile bio
- Share with recruiters
- Include in job applications
- Use in portfolio

**Format for README:**
```markdown
## Live Demo

Try NoDetours live: **[https://nodetours.onrender.com](https://nodetours.onrender.com)**

Example query: "Help me plan a 7-day trip to Japan focusing on anime sites and local food"
```

---

## Monitoring Your Deployment

### In Render Dashboard

**Logs Tab**:
- See real-time application logs
- Useful for debugging errors
- Shows API calls and responses

**Metrics Tab** (if available):
- CPU usage (should be <30% at rest)
- Memory usage (should be <300 MB)
- Response times
- Request count

**Auto-Deploy**:
- Set to "Yes" (default)
- Redeploys automatically when you push to GitHub
- No manual redeployment needed

---

## Cost Breakdown

### Render Hosting
- **Free Tier**: $0 (but services spin down after 15 min inactivity)
- **Starter Plan**: $7/month (recommended for demo)
- **Standard Plan**: $12/month (for production)

### API Costs (Monthly, ~100 requests/day)
- **OpenAI GPT-4**: ~$390/month
- **Anthropic Claude**: ~$78/month ✅ **Best Value**
- **OpenAI GPT-3.5**: ~$9/month
- **Other APIs**: ~$20-30/month

**Recommended Monthly Cost**: $12 (Render) + $78 (Claude) = **~$90/month**

---

## Common Customizations

### Change Default LLM Provider
Edit `config/config.yaml`:
```yaml
llm_providers:
  default: "claude"  # or "gpt4", "gpt35"
```
Push to GitHub → auto-redeploys

### Adjust Response Timeout
Edit `Procfile`:
```
web: gunicorn -w 4 -b 0.0.0.0:$PORT --timeout 180 api.app:app
```
Change `180` to your desired seconds. Push to GitHub.

### Add Custom Domain
In Render dashboard:
1. Settings → Custom Domain
2. Enter your domain (e.g., `travel.example.com`)
3. Update DNS records per Render instructions
4. HTTPS auto-enabled

---

## Advanced: Auto-Scaling

Already configured in `render.yaml`:
```yaml
scaling:
  minInstances: 1      # Always 1 running
  maxInstances: 3      # Auto-scale to 3 if needed
```

**How it works**:
- 1-100 requests/day: 1 instance (saves cost)
- 100-1000 requests/day: Still 1 instance (sufficient)
- 1000+ requests/day: Auto-scales to 2-3 instances
- Cost automatically increases with usage

---

## After Deployment: Next Steps

### Immediate (Today)
- [ ] Test application with sample queries
- [ ] Verify all features work
- [ ] Update README with live demo link
- [ ] Share URL with friends/colleagues

### This Week
- [ ] Monitor performance metrics
- [ ] Check logs for any errors
- [ ] Optimize if needed (see BENCHMARKS.md)
- [ ] Update GitHub bio with deployment

### Next Week
- [ ] Gather feedback from testers
- [ ] Write blog post about deployment
- [ ] Share in job applications
- [ ] Use in interviews as portfolio piece

---

## Support & Help

**If something goes wrong:**
1. Check Logs tab in Render dashboard
2. Read DEPLOYMENT.md troubleshooting section
3. See TECHNICAL_DECISIONS.md for architecture context
4. Check Render docs: https://render.com/docs

**For code issues:**
- Read CONTRIBUTING.md for development setup
- Open issue on GitHub
- Check existing issues for solutions

---

## Summary: You're Done! 🎉

| Step | Time | Status |
|------|------|--------|
| Get API Keys | 5 min | ✓ Done |
| Commit to GitHub | 2 min | ✓ Done |
| Create Render Service | 5 min | ⏳ Next |
| Add Environment Variables | 5 min | ⏳ Next |
| Verify Deployment | 3 min | ⏳ Next |
| **Total Time** | **20 min** | **🚀 Live!** |

Your NoDetours application is ready to deploy. Follow the steps above, and you'll have a live, production-ready AI travel planner in under 20 minutes.

**Questions?** Check DEPLOYMENT.md for detailed documentation.

**Ready to deploy?** Follow Step 1 above and you'll be live within 15 minutes! 🚀
