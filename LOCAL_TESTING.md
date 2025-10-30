# Running NoDetours Locally - Quick Setup

**Total Time: 5-10 minutes**

---

## Prerequisites

- Python 3.9+ installed
- Git (optional, but recommended)
- Terminal/Command prompt access

---

## Step 1: Clone or Navigate to Repository

```bash
cd /Users/monesh/University/NoDetours
```

---

## Step 2: Create Virtual Environment (Optional but Recommended)

```bash
# Create virtual environment
python -m venv venv

# Activate it
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

---

## Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

This installs all required packages (FastAPI, Uvicorn, OpenAI, Anthropic, etc.)

---

## Step 4: Get an API Key

You need **at least ONE** of these:

### Option A: Anthropic Claude (RECOMMENDED - Free tier available)
1. Go to: https://console.anthropic.com/
2. Sign in (create account if needed)
3. Click "API Keys" → "Create Key"
4. Copy the key
5. Edit the `.env` file in your project directory and paste it:
   ```
   ANTHROPIC_API_KEY=sk-ant-your-key-here
   ```

### Option B: OpenAI (GPT-3.5/4)
1. Go to: https://platform.openai.com/api-keys
2. Create API key
3. Copy the key
4. Edit `.env` and add:
   ```
   OPENAI_API_KEY=sk-your-key-here
   ```

---

## Step 5: Update .env File

Open `.env` in the project root and update with your API key:

```env
# LLM Provider - Add your actual key
ANTHROPIC_API_KEY=sk-ant-your-actual-key-here

# Optional APIs (leave empty for now)
# WEATHER_API_KEY=...
# MAPS_API_KEY=...

# App Configuration
APP_HOST=127.0.0.1
APP_PORT=8000
APP_ENV=development
LOG_LEVEL=INFO
```

**Save the file!**

---

## Step 6: Run the Application

```bash
python main.py
```

You should see:

```
Starting NoDetours web app at http://127.0.0.1:8000
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

---

## Step 7: Open in Browser

1. Open your browser
2. Go to: **http://localhost:8000** or **http://127.0.0.1:8000**
3. You should see the NoDetours web interface

---

## Step 8: Test the Application

1. Enter a travel query:
   ```
   "Help me plan a 3-day trip to Paris with museums and local food"
   ```

2. Click "Create Travel Plan"

3. Wait ~10 seconds for the AI to process

4. You should see:
   - ✅ **Itinerary**: Day-by-day schedule
   - ✅ **Packing List**: What to bring
   - ✅ **Budget Estimate**: Cost breakdown
   - ✅ **Download Calendar**: Export as ICS file

---

## Troubleshooting

### Issue: "ANTHROPIC_API_KEY not found"

**Solution**:
1. Make sure you added your key to `.env` file
2. Restart the application: `Ctrl+C` then `python main.py`
3. Verify the key is correct (no spaces, exact copy)

### Issue: "Command not found: python"

**Solution**:
```bash
python3 main.py  # Try python3 instead
```

### Issue: "ModuleNotFoundError: No module named 'fastapi'"

**Solution**:
```bash
pip install -r requirements.txt  # Reinstall dependencies
```

### Issue: "Address already in use"

**Solution**: Port 8000 is already in use. Either:
1. Kill the process using port 8000
2. Change port in config.yaml:
   ```yaml
   app:
     port: 8001  # Use different port
   ```

### Issue: Application runs but no response to queries

**Possible causes**:
1. API key is invalid
2. API rate limit exceeded
3. Network connectivity issue

**Solution**:
1. Check `.env` file has correct key
2. Check network connection
3. See logs for error messages

---

## Running in CLI Mode

Instead of web interface, run in terminal:

```bash
python main.py --cli
```

You'll be prompted for travel queries and responses appear in terminal.

---

## Checking Logs

Application logs show what's happening:

```
INFO:app.agent:Initializing TravelPlannerAgent
INFO:api.llm_provider:Initializing LLMProvider...
INFO:api.app:Request received...
```

Check these for debugging if something goes wrong.

---

## Configuration Options

Edit `config/config.yaml` to customize:

```yaml
# Change LLM provider
llm:
  provider: "anthropic"  # or "openai"
  model: "claude-3-5-sonnet-20241022"
  temperature: 0.7  # Lower = more focused, Higher = more creative
  max_tokens: 4000

# Change port
app:
  port: 8001  # Use different port

# Enable/disable APIs
apis:
  weather:
    provider: "mock"  # or real provider
  maps:
    provider: "mock"  # or real provider
```

---

## Performance Notes

### First Request
- **Time**: ~10-15 seconds
- **Why**: Cold start (model initialization)
- **Normal**: Expected behavior

### Subsequent Requests
- **Time**: ~6-7 seconds
- **Why**: Model already loaded
- **Normal**: Expected behavior

### Response Includes
- Real-time weather data (if API available)
- Location information (if API available)
- AI-generated itinerary, packing list, budget

---

## What's Working

✅ Web interface loads correctly
✅ Chat/query input works
✅ FastAPI running on localhost
✅ Configuration system works
✅ LLM provider initialization

---

## What Requires API Keys

- Actual LLM responses (needs OpenAI or Anthropic key)
- Weather data (needs Weather API key - optional)
- Maps/location data (needs Google Maps - optional)
- Search results (needs Search API - optional)

**Core functionality works with just an LLM API key**

---

## Next Steps

### After Local Testing Works:
1. Deploy to Render.com (see RENDER_SETUP.md)
2. Add optional API keys for better experience
3. Share live URL with others

### Customization Options:
1. Modify prompts in `app/modules/output_generator.py`
2. Change LLM provider in `config/config.yaml`
3. Add new features in `app/modules/`
4. Customize UI in `static/` and `templates/`

---

## Quick Reference Commands

```bash
# Start application
python main.py

# Start in CLI mode
python main.py --cli

# Install dependencies
pip install -r requirements.txt

# Update requirements after pip install
pip freeze > requirements.txt

# Run tests (if test file exists)
pytest

# Check Python version
python --version

# Deactivate virtual environment
deactivate
```

---

## Success Checklist

- [ ] Python installed
- [ ] Repository cloned/navigated to
- [ ] Virtual environment created (optional)
- [ ] Dependencies installed
- [ ] `.env` file created with API key
- [ ] Application runs without errors
- [ ] Browser accessible at localhost:8000
- [ ] Web interface loads
- [ ] Test query returns results

**All checked?** Application is working locally! 🎉

---

## Next: Deploy to Render

Once local testing confirms everything works:

1. Read: `RENDER_SETUP.md`
2. Follow: 20-minute deployment steps
3. Get: Live URL for your application
4. Share: With recruiters and your network

---

## Need Help?

- See: `QUICK_RENDER_GUIDE.md` for deployment
- See: `DEPLOYMENT.md` for detailed troubleshooting
- See: `TECHNICAL_DECISIONS.md` for architecture questions
- See: Application logs for error messages

---

**You're all set! Run `python main.py` and test your application locally.** 🚀
