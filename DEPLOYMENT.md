# Deployment Guide for NoDetours

This guide provides step-by-step instructions for deploying NoDetours to Render.com and other platforms.

---

## Quick Start: Deployment to Render.com

### Prerequisites
1. GitHub account with NoDetours repository pushed
2. Render.com account (free tier available at render.com)
3. API keys for:
   - OpenAI (or Anthropic, or both)
   - Weather API
   - Google Maps API
   - Search API (Google Custom Search)
   - FireCrawl (optional but recommended)

### Step 1: Prepare Repository

Ensure your repository has these files (already included):
- ✅ `render.yaml` - Render deployment configuration
- ✅ `Procfile` - Process file for Render
- ✅ `requirements.txt` - Python dependencies with versions pinned
- ✅ `.env.example` - Environment variable template

### Step 2: Create Render Account & Connect Repository

1. Go to [render.com](https://render.com)
2. Click "Sign up" (recommended: sign up with GitHub)
3. Connect your GitHub account
4. Authorize Render to access your repositories
5. Select the NoDetours repository

### Step 3: Create New Web Service

1. In Render dashboard, click "New +" → "Web Service"
2. Select your NoDetours repository
3. Configure service:

| Setting | Value |
|---------|-------|
| Name | `nodetours` |
| Region | `oregon` (or closest to you) |
| Runtime | `Python 3` |
| Build Command | `pip install -r requirements.txt` |
| Start Command | `gunicorn -w 4 -b 0.0.0.0:$PORT --timeout 120 api.app:app` |
| Instance Type | `Starter` (for development/demo) or `Standard` (for production) |

4. Click "Create Web Service"

### Step 4: Add Environment Variables

In the Render dashboard for your service:

1. Go to "Environment" tab
2. Add these variables (copy from your local `.env` file):

```
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
WEATHER_API_KEY=...
MAPS_API_KEY=...
SEARCH_API_KEY=...
SEARCH_ENGINE_ID=...
FIRECRAWL_API_KEY=...
LOG_LEVEL=INFO
APP_ENV=production
```

**Important**: Set `sync: false` in render.yaml for these variables—they're secrets that shouldn't be in version control.

### Step 5: Deploy

1. Click "Deploy" button
2. Wait for build to complete (typically 3-5 minutes)
3. Once deployed, get your service URL from the Render dashboard
4. Click the URL to access your NoDetours application

### Step 6: Verify Deployment

Access your deployed app:
1. Open the Render-provided URL (e.g., `https://nodetours.onrender.com`)
2. Test the travel planning feature with a sample query
3. Verify all features work (itinerary, packing list, budget)

### Step 7: Monitor Deployment

In Render dashboard:
- **Logs**: View application logs for debugging
- **Metrics**: Monitor CPU, memory, and request counts
- **Deployments**: See deployment history and roll back if needed

---

## Configuration Details

### render.yaml Explained

```yaml
# Python version and runtime
runtime: python
pythonVersion: 3.9

# Build steps
buildCommand: pip install --upgrade pip && pip install -r requirements.txt

# How to start the application
# Gunicorn: Production WSGI server
# -w 4: 4 worker processes
# --timeout 120: 120 second timeout for long requests
startCommand: gunicorn -w 4 -b 0.0.0.0:$PORT --timeout 120 api.app:app

# Port configuration
port: 8000

# Health check endpoint
healthCheckPath: /

# Scaling configuration
scaling:
  minInstances: 1      # Minimum instances always running
  maxInstances: 3      # Auto-scale up to 3 instances
  targetMemoryUtilization: 70  # Scale up if memory > 70%
  targetCpuUtilization: 70     # Scale up if CPU > 70%
```

### Environment Variables Reference

| Variable | Purpose | Example |
|----------|---------|---------|
| `OPENAI_API_KEY` | OpenAI API access (GPT-3.5/4) | `sk-...` |
| `ANTHROPIC_API_KEY` | Anthropic Claude API access | `sk-ant-...` |
| `WEATHER_API_KEY` | Weather forecast data | API key from weatherapi.com |
| `MAPS_API_KEY` | Location and maps data | Google Maps API key |
| `SEARCH_API_KEY` | Web search integration | Google Search API key |
| `SEARCH_ENGINE_ID` | Custom Search Engine ID | From cse.google.com |
| `FIRECRAWL_API_KEY` | Web scraping capability | API key from firecrawl.dev |
| `LOG_LEVEL` | Logging verbosity | `INFO` or `DEBUG` |
| `APP_ENV` | Environment context | `production`, `staging`, `development` |

---

## Obtaining API Keys

### OpenAI (GPT-3.5/4)
1. Go to https://platform.openai.com/api-keys
2. Sign in or create account
3. Create new API key
4. Copy key (shown once only)
5. Set billing (required for API access)

### Anthropic (Claude)
1. Go to https://console.anthropic.com/
2. Sign in or create account
3. Navigate to API keys section
4. Create new API key
5. Copy key to your environment variables

### Weather API
1. Go to https://www.weatherapi.com/
2. Sign up (free tier available)
3. Get API key from dashboard
4. Set up in your environment

### Google Maps API
1. Go to https://console.cloud.google.com/
2. Create new project
3. Enable "Maps JavaScript API"
4. Create API key (Application restrictions recommended)
5. Set up billing

### Google Custom Search Engine
1. Go to https://cse.google.com/cse/
2. Create new search engine
3. Get API key from https://console.developers.google.com/
4. Set up billing

### FireCrawl (Web Scraping)
1. Go to https://www.firecrawl.dev/
2. Sign up
3. Get API key from dashboard
4. (Optional - can use web scraping without this)

---

## Monitoring & Troubleshooting

### Check Logs

In Render dashboard:
```
Logs tab → View real-time application logs
```

Common log messages:
```
INFO:     Application startup complete
```
✅ Application started successfully

```
ERROR: Failed to connect to OpenAI API
```
❌ Check `OPENAI_API_KEY` is set correctly

```
Timeout waiting for response
```
⚠️ Increase timeout in render.yaml if needed

### Monitor Metrics

In Render dashboard:
- **Memory Usage**: Should stay < 500 MB
- **CPU Usage**: Should average < 30%
- **Response Time**: p95 should be < 10 seconds

### Common Issues & Solutions

**Issue**: Build fails with "module not found"
```
Solution: Ensure all dependencies are in requirements.txt
Run locally: pip install -r requirements.txt
```

**Issue**: Application times out on first request
```
Solution: First request includes cold start
Increase timeout in render.yaml: --timeout 120
Normal behavior, subsequent requests are faster
```

**Issue**: API key errors
```
Solution: Verify keys in Render dashboard
Check keys are not truncated
Ensure no extra spaces in values
```

**Issue**: Weather/Maps API not working
```
Solution: Verify API key is active
Check API limits/quotas
Verify geographic region is supported
Application gracefully degrades if API unavailable
```

---

## Cost Considerations

### Render.com Pricing
- **Starter Plan**: $7/month (basic instance)
- **Standard Plan**: $12/month (recommended)
- **Auto-scaling**: Additional instances charged per tier

For typical usage:
- 100 requests/day: Starter plan sufficient
- 1000 requests/day: Standard plan recommended
- 10000+ requests/day: Consider higher tier or caching

### API Costs (Per Request)

| Provider | Cost | Notes |
|----------|------|-------|
| OpenAI GPT-4 | ~$0.13 | Higher quality, higher cost |
| Anthropic Claude | ~$0.026 | Best value (recommended) |
| OpenAI GPT-3.5 | ~$0.003 | Cheapest, lower quality |
| Weather API | Free tier + paid | Usually <$1/month |
| Google Maps | Free tier | 1000 requests/day free |
| Search API | Paid | ~$100/month for 10k searches |

**Monthly Cost Estimate** (100 requests/day with Anthropic Claude):
```
Render hosting: $12
API calls: 3,000 × $0.026 = $78
Other APIs: ~$20
─────────────────────────
Total: ~$110/month
```

---

## Performance Optimization

### Caching Strategy
- Implement Redis caching for popular destinations
- Cache weather data for 6-12 hours
- Cache search results for 24 hours

### Scaling Strategy
```
0-100 requests/day    → 1 Starter instance
100-1000 requests/day → 1 Standard instance
1000-5000 requests/day → 2-3 Standard instances
5000+ requests/day    → Consider database + advanced caching
```

### Response Time Optimization
1. **Parallel API calls**: Already implemented (2.1s for 5+ APIs)
2. **Prompt optimization**: Fine-tune LLM prompts to reduce output tokens
3. **Streaming responses**: Implement server-sent events (SSE) for real-time output
4. **Caching**: Implement Redis for frequently accessed data

---

## Production Checklist

Before deploying to production:

- [ ] All API keys obtained and working locally
- [ ] Environment variables set in Render dashboard
- [ ] Health check endpoint (`/`) responsive
- [ ] Error handling tested (API timeouts, missing data)
- [ ] Logging configured appropriately
- [ ] Database configured (if using persistent storage)
- [ ] Backup strategy defined
- [ ] Monitoring/alerting set up
- [ ] Rate limiting configured (if needed)
- [ ] HTTPS enabled (Render provides by default)
- [ ] Documentation updated with live URL
- [ ] Team notified of deployment

---

## Updating Deployment

### Deploying Code Changes

1. Push changes to GitHub:
```bash
git add .
git commit -m "Feature: add new capability"
git push origin main
```

2. Render automatically deploys (if auto-deploy enabled)
3. Monitor deployment in Render dashboard

### Updating Environment Variables

1. In Render dashboard
2. Go to "Environment" tab
3. Update variable value
4. Click "Save"
5. Service auto-restarts with new variables

### Rolling Back to Previous Deployment

1. In Render dashboard → "Deployments" tab
2. Find previous successful deployment
3. Click "Redeploy"
4. Confirm rollback

---

## Advanced Configuration

### Custom Domain
1. In Render dashboard → "Settings" → "Custom Domain"
2. Add your domain
3. Update DNS records per Render instructions
4. Enable HTTPS (automatic)

### Persistent Storage
Already configured in render.yaml:
```yaml
disk:
  name: nodetours-data
  mountPath: /data
  sizeGB: 1
```

Use for: Evaluation results, logs, user data

### Scheduled Jobs (Evaluation Runs)
Uncomment in render.yaml to run evaluations weekly:
```yaml
cronJobs:
  - name: weekly-evaluation
    schedule: "0 2 * * 0"
    command: python run_evaluation.py
```

---

## Support & Documentation

- **Render Docs**: https://render.com/docs
- **FastAPI Docs**: https://fastapi.tiangolo.com/
- **Gunicorn Docs**: https://gunicorn.org/
- **NoDetours Issues**: Use GitHub Issues for problems

---

## Next Steps After Deployment

1. **Test the Application**: Verify all features work
2. **Monitor Metrics**: Check Render dashboard regularly
3. **Collect Feedback**: Get user feedback on quality
4. **Optimize**: Based on metrics, optimize code/prompts
5. **Update Documentation**: Share live URL in README

---

**Your NoDetours app is now live! 🚀**

Share your deployment URL:
- Add to README.md: `[Live Demo](your-url)`
- Update GitHub profile bio
- Share with recruiters and team members
- Use as portfolio piece

For questions, check CONTRIBUTING.md or open an issue on GitHub.
