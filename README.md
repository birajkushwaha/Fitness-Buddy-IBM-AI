# 🏋️ Fitness Buddy AI — IBM watsonx.ai Powered Personal Trainer

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue?logo=python" />
  <img src="https://img.shields.io/badge/Flask-3.0.3-black?logo=flask" />
  <img src="https://img.shields.io/badge/IBM%20watsonx.ai-1.5.14-054ADA?logo=ibm" />
  <img src="https://img.shields.io/badge/Bootstrap-5.3-purple?logo=bootstrap" />
  <img src="https://img.shields.io/badge/License-MIT-green" />
</p>

A full-stack AI fitness web application built with **Python Flask** and **IBM watsonx.ai**. Fitness Buddy provides personalized workout plans, nutrition advice, fitness calculators, progress tracking, and a real-time AI chat coach — all powered by large language models running on IBM Cloud.

---

## 📸 Features

| Category | Details |
|---|---|
| 🤖 **AI Coach** | Real-time chat — workout plans, meal advice, motivation, exercise tips |
| 🏋️ **Workout Planner** | AI-generated 7-day plans by goal, level, session time & equipment |
| 🥗 **Nutrition** | Daily meal plans with macros, calorie targets & diet preferences |
| 📊 **Calculators** | BMI, TDEE/Calories (Mifflin-St Jeor), Daily Water Intake |
| 📈 **Progress Tracking** | Log workouts, weight, notes; streak counter; achievements system |
| 👤 **User Profile** | Persistent profile that personalizes every AI response |
| 🌙 **Dark Mode** | Full dark/light toggle with smooth transitions |
| 📱 **Responsive** | Mobile-first Bootstrap 5 layout, works on all screen sizes |
| ⚠️ **Demo Mode** | Calculators & progress tracking work even without IBM credentials |

---

## 🗂️ Project Structure

```
fitness-buddy/
├── app.py                  # Flask backend + IBM watsonx.ai integration
├── requirements.txt        # Python dependencies
├── .env.example            # Credential template (copy to .env)
├── .gitignore              # Excludes .env and .venv from git
├── templates/
│   └── index.html          # Single-page application HTML
└── static/
    ├── css/
    │   └── style.css       # All styles, dark mode, animations
    └── js/
        └── app.js          # All frontend logic (navigation, API calls, storage)
```

---

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/birajkushwaha/Fitness-Buddy-IBM-Watsonx.git
cd Fitness-Buddy-IBM-Watsonx
```

### 2. Create a Python virtual environment

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure IBM watsonx.ai credentials

```bash
# Windows
copy .env.example .env

# macOS / Linux
cp .env.example .env
```

Open `.env` and fill in your values:

```env
IBM_API_KEY=your_ibm_cloud_api_key_here
WATSONX_PROJECT_ID=your_watsonx_project_id_here
WATSONX_URL=https://au-syd.ml.cloud.ibm.com
FLASK_SECRET_KEY=any_long_random_string
FLASK_ENV=development
```

> **How to get your credentials:**
>
> **IBM Cloud API Key**
> 1. Log in to [cloud.ibm.com](https://cloud.ibm.com)
> 2. Go to **Manage → Access (IAM) → API keys**
> 3. Click **"Create an IBM Cloud API key"** → copy the key immediately
>
> **watsonx.ai Project ID**
> 1. Go to [dataplatform.cloud.ibm.com](https://dataplatform.cloud.ibm.com)
> 2. Open your watsonx.ai project → **Manage** tab → **General**
> 3. Copy the **Project ID** (UUID format)

### 5. Run the application

```bash
python app.py
```

Open your browser at **http://localhost:5000** 🎉

---

## 🌏 Supported IBM watsonx.ai Regions

Set `WATSONX_URL` in your `.env` to the region where your project is hosted:

| Region | URL |
|---|---|
| **Sydney** (au-syd) | `https://au-syd.ml.cloud.ibm.com` |
| Dallas (us-south) | `https://us-south.ml.cloud.ibm.com` |
| Frankfurt (eu-de) | `https://eu-de.ml.cloud.ibm.com` |
| London (eu-gb) | `https://eu-gb.ml.cloud.ibm.com` |
| Tokyo (jp-tok) | `https://jp-tok.ml.cloud.ibm.com` |
| Toronto (ca-tor) | `https://ca-tor.ml.cloud.ibm.com` |

---

## 🤖 AI Model

Default model: **`meta-llama/llama-3-3-70b-instruct`**

The model used is determined by what is available in your watsonx.ai project and region. To switch, change `MODEL_ID` in `app.py`:

```python
MODEL_ID = "meta-llama/llama-3-3-70b-instruct"   # default (Sydney)
# MODEL_ID = "ibm/granite-3-1-8b-base"            # IBM Granite alternative
# MODEL_ID = "ibm/granite-8b-code-instruct"        # Code-focused Granite
```

> **Tip:** To see which models are available in your project, the app logs the supported model list if initialization fails with an unsupported model error.

### Generation Parameters

Configured in `app.py` using the typed `TextGenParameters` schema:

```python
GENERATION_PARAMS = TextGenParameters(
    temperature=0.7,        # creativity (0 = deterministic, 1 = creative)
    top_p=0.9,              # nucleus sampling
    top_k=50,               # top-k sampling
    repetition_penalty=1.1, # reduce repeated phrases
)
```

---

## 🤖 Customizing the AI Agent

The AI persona and behaviour are controlled by the `SYSTEM_PROMPT` variable in `app.py`. The full set of instructions is documented in the `AGENT_INSTRUCTIONS` block at the top of that file.

| Section | What to customize |
|---|---|
| **TONE & PERSONALITY** | Communication style, encouragement level |
| **WORKOUT SPECIALIZATION** | Default exercise splits, warm-up rules, home vs gym |
| **SAFETY RULES** | Guardrails — never remove rule 1 (doctor consultation) |
| **FITNESS PREFERENCES** | Default cardio targets, hydration formula, meal philosophy |
| **RESPONSE FORMAT** | Structure of workout/meal plan output |

---

## 🔌 API Reference

All endpoints accept and return JSON.

| Endpoint | Method | Description |
|---|---|---|
| `/` | GET | Serve the single-page application |
| `/api/chat` | POST | AI chat with conversation history |
| `/api/workout-plan` | POST | Generate a 7-day AI workout plan |
| `/api/meal-plan` | POST | Generate a personalized daily meal plan |
| `/api/calculate-bmi` | POST | BMI + category + advice |
| `/api/calculate-calories` | POST | TDEE + macro split (Mifflin-St Jeor) |
| `/api/calculate-water` | POST | Daily water intake recommendation |
| `/api/motivation` | GET | Single motivational quote from AI |
| `/api/health` | GET | Health check + `ai_ready` status + region |

### Example requests

**Chat:**
```bash
curl -X POST http://localhost:5000/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Create a 30-minute home workout for weight loss",
    "profile": {"weight": 80, "height": 175, "age": 30, "gender": "male", "goal": "weight_loss"},
    "history": []
  }'
```

**BMI Calculator:**
```bash
curl -X POST http://localhost:5000/api/calculate-bmi \
  -H "Content-Type: application/json" \
  -d '{"weight": 75, "height": 175}'
```

**Health check:**
```bash
curl http://localhost:5000/api/health
# {"ai_ready": true, "model": "meta-llama/llama-3-3-70b-instruct", "region": "https://au-syd.ml.cloud.ibm.com", ...}
```

---

## 🌐 Deployment

### Gunicorn (Linux / macOS production)

```bash
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

### Docker

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
```

```bash
docker build -t fitness-buddy-ai .
docker run -p 5000:5000 --env-file .env fitness-buddy-ai
```

### Render / Railway / Fly.io

1. Connect your GitHub repository
2. Set environment variables from `.env.example` in the platform dashboard
3. Set the start command to:
   ```
   gunicorn -w 2 -b 0.0.0.0:$PORT app:app
   ```

### IBM Code Engine

```bash
ibmcloud ce project select --name my-project
ibmcloud ce app create \
  --name fitness-buddy-ai \
  --image icr.io/<namespace>/fitness-buddy-ai \
  --port 5000 \
  --env-from-secret fitness-buddy-secrets
```

---

## 📦 Dependencies

| Package | Version | Purpose |
|---|---|---|
| Flask | 3.0.3 | Web framework |
| Flask-CORS | 4.0.1 | Cross-origin resource sharing |
| python-dotenv | 1.0.1 | Load `.env` credentials |
| ibm-watsonx-ai | 1.5.14 | IBM watsonx.ai Python SDK |
| requests | 2.32.3 | HTTP client |
| gunicorn | 22.0.0 | Production WSGI server |

Frontend uses **Bootstrap 5.3** and **Bootstrap Icons 1.11** via CDN — no npm install needed.

---

## 🐛 Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| Demo Mode banner appears | Credentials missing or invalid | Check `.env` — ensure `IBM_API_KEY` and `WATSONX_PROJECT_ID` are set |
| `BXNIM0415E: API key could not be found` | API key deleted or incorrect | Generate a new key at [cloud.ibm.com/iam/apikeys](https://cloud.ibm.com/iam/apikeys) |
| `Model X is not supported for this environment` | Model not deployed in your region/project | Change `MODEL_ID` in `app.py` to one from the supported list printed in the error |
| `The specified url is not valid` | SDK version too old or wrong URL | Ensure `ibm-watsonx-ai==1.5.14` is installed and URL matches your region |
| `404 Project not found` | Wrong project ID | Copy Project ID from watsonx.ai project → Manage → General |
| Port already in use | Another process on port 5000 | Run `PORT=5001 python app.py` |

---

## 🔒 Security

- `.env` is in `.gitignore` — your credentials are **never committed**
- Rotate your IBM Cloud API Key regularly from [cloud.ibm.com/iam/apikeys](https://cloud.ibm.com/iam/apikeys)
- Use a strong random `FLASK_SECRET_KEY` in production
- Set `FLASK_ENV=production` for production deployments
- The `ModelInference` client is initialized **once at startup** — no credentials are re-read per request

---
## 👨‍💻 Developer

**Biraj Kushwaha**

---

## 📄 License

MIT — free to use, modify, and deploy.

---

*Built with ❤️ using IBM watsonx.ai · Python Flask · Bootstrap 5*
