# 📚 Course Content Simplification Agent

> An AI-powered web application that transforms complex academic content into
> clear, easy-to-understand language — powered by **IBM Granite Foundation Models**
> on **IBM watsonx.ai**.

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-3.0-green?logo=flask)
![IBM watsonx](https://img.shields.io/badge/IBM-watsonx.ai-0062FF?logo=ibm)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-purple?logo=bootstrap)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **IBM Granite AI** | Uses `ibm/granite-13b-instruct-v2` for high-quality simplification |
| 📄 **Content Simplification** | Transforms dense academic text into clear explanations |
| 📁 **PDF & Text Upload** | Upload PDFs, TXT, or MD files and simplify instantly |
| 💬 **Chat Interface** | Multi-turn conversation with the AI tutor |
| 🎯 **3 Learning Levels** | Beginner → Intermediate → Advanced |
| 📝 **Structured Output** | Summary, Key Concepts, Keywords, Revision Notes, Practice Q&A |
| 📊 **Dashboard** | Track all documents and simplification history |
| 🌙 **Dark Mode** | Full dark/light theme with localStorage persistence |
| 📱 **Mobile Responsive** | Works beautifully on all screen sizes |
| 🔒 **Secure** | Environment-based credential management, no secrets in code |

---

## 🗂️ Project Structure

```
course-simplifier/
├── app.py                    # Main Flask application & AGENT_INSTRUCTIONS
├── requirements.txt          # Python dependencies
├── .env.example              # Environment variable template
├── .gitignore                # Files excluded from version control
├── Procfile                  # Heroku/Cloud Foundry process definition
├── runtime.txt               # Python runtime version
├── app.json                  # Heroku app metadata
├── README.md                 # This file
│
├── templates/
│   ├── base.html             # Shared layout: nav, footer, toast container
│   ├── index.html            # Landing page with live demo card
│   ├── chat.html             # Full chat interface with sidebar
│   ├── dashboard.html        # Documents & recent simplifications
│   ├── history.html          # Full simplification history with search
│   └── profile.html          # User preferences & settings
│
└── static/
    ├── css/
    │   ├── main.css          # Design tokens, components, dark mode, animations
    │   └── chat.css          # Chat-specific layout and message styles
    ├── js/
    │   ├── main.js           # Theme, toasts, health check, scroll reveal
    │   ├── chat.js           # Chat logic, message rendering, export
    │   └── upload.js         # Drag-and-drop file handling
    └── images/               # Static assets placeholder
```

---

## 🚀 Quick Start

### Prerequisites

- Python **3.11+**
- An **IBM Cloud** account → [Sign up free](https://cloud.ibm.com/registration)
- A **watsonx.ai** project → [Create one](https://dataplatform.cloud.ibm.com/)
- An **IBM Cloud API key** → [Generate here](https://cloud.ibm.com/iam/apikeys)

---

### 1 · Clone & Install

```bash
# Clone the repository
git clone https://github.com/your-username/course-simplifier.git
cd course-simplifier

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate          # macOS / Linux
# venv\Scripts\activate           # Windows

# Install dependencies
pip install -r requirements.txt
```

### 2 · Configure Environment Variables

```bash
# Copy the example file
cp .env.example .env

# Edit .env with your IBM credentials
nano .env          # or use VS Code, vim, etc.
```

Fill in these values in `.env`:

```env
WATSONX_API_KEY=your_ibm_cloud_api_key
WATSONX_PROJECT_ID=your_watsonx_project_id
WATSONX_URL=https://us-south.ml.cloud.ibm.com
GRANITE_MODEL_ID=ibm/granite-13b-instruct-v2
SECRET_KEY=your-random-secret-key-here
```

> 💡 **Generate a strong secret key:**
> ```bash
> python -c "import secrets; print(secrets.token_hex(32))"
> ```

### 3 · Run the Application

```bash
python app.py
```

Open your browser at **http://localhost:5000** 🎉

---

## 🔧 Configuration Reference

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `WATSONX_API_KEY` | ✅ Yes | — | IBM Cloud API key |
| `WATSONX_PROJECT_ID` | ✅ Yes | — | watsonx.ai Project ID |
| `WATSONX_URL` | No | `https://us-south.ml.cloud.ibm.com` | Regional endpoint |
| `GRANITE_MODEL_ID` | No | `ibm/granite-13b-instruct-v2` | Granite model to use |
| `MAX_NEW_TOKENS` | No | `1024` | Max tokens per response |
| `SECRET_KEY` | ✅ Yes (prod) | `dev-secret-change-me` | Flask session key |
| `FLASK_DEBUG` | No | `false` | Enable debug mode |
| `PORT` | No | `5000` | Listening port |

### Available IBM Granite Models

| Model ID | Best For |
|----------|----------|
| `ibm/granite-13b-instruct-v2` | General instruction-following (recommended) |
| `ibm/granite-3-8b-instruct` | Fast responses, lower latency |
| `ibm/granite-20b-multilingual` | Multilingual content |

---

## 🤖 Customising the Agent

The agent's behaviour is fully controlled by the `AGENT_INSTRUCTIONS` dictionary
at the top of [`app.py`](app.py). Modify it to change:

```python
AGENT_INSTRUCTIONS = {
    # Communication tone
    "communication_style": "friendly and educational",

    # Per-level instructions
    "levels": {
        "beginner":     "Use very simple words...",
        "intermediate": "Use clear language...",
        "advanced":     "Retain domain-specific terminology...",
    },

    # Output format sections
    "output_format": "1. Summary\n2. Key Concepts\n...",

    # Safety rules list
    "safety_rules": [...],

    # Academic integrity statement
    "academic_integrity": "Always encourage original thinking...",

    # Language code
    "language": "en",
}
```

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/simplify` | Simplify pasted text content |
| `POST` | `/api/chat` | Send a chat message |
| `POST` | `/api/upload` | Upload and simplify a file |
| `POST` | `/api/clear-history` | Clear chat history |
| `POST` | `/api/preferences` | Save user preferences |
| `GET`  | `/api/health` | Health check |

### Example: Simplify Text

```bash
curl -X POST http://localhost:5000/api/simplify \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Mitochondria are membrane-bound organelles...",
    "level": "beginner"
  }'
```

Response:
```json
{
  "result": "**Summary**\nMitochondria are the power generators...",
  "id": "uuid-string"
}
```

---

## ☁️ Deployment

### Deploy to Heroku

```bash
# Install Heroku CLI and login
heroku login

# Create app
heroku create your-app-name

# Set environment variables
heroku config:set WATSONX_API_KEY=your_key
heroku config:set WATSONX_PROJECT_ID=your_project_id
heroku config:set SECRET_KEY=$(python -c "import secrets; print(secrets.token_hex(32))")

# Deploy
git push heroku main

# Open app
heroku open
```

### Deploy to IBM Code Engine

```bash
# Build and push Docker image
ibmcloud ce application create \
  --name course-simplifier \
  --image icr.io/your-namespace/course-simplifier:latest \
  --env WATSONX_API_KEY=your_key \
  --env WATSONX_PROJECT_ID=your_project_id \
  --port 5000
```

### Deploy with Docker

```dockerfile
# Create Dockerfile in project root:
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["gunicorn", "app:app", "--workers", "2", "--bind", "0.0.0.0:5000"]
```

```bash
docker build -t course-simplifier .
docker run -p 5000:5000 --env-file .env course-simplifier
```

---

## 🛡️ Security Best Practices

1. **Never commit `.env`** – it's in `.gitignore` by default
2. **Use a strong `SECRET_KEY`** in production (32+ random bytes)
3. **Rotate API keys** regularly via IBM Cloud IAM
4. **Set `FLASK_DEBUG=false`** in all production environments
5. **Use HTTPS** – configure your reverse proxy (nginx/Heroku) for TLS

---

## 🧪 Running Tests (Optional)

```bash
pip install pytest
pytest tests/          # if you add a tests/ directory
```

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m "Add amazing feature"`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📞 Support

- 📖 [IBM watsonx.ai Documentation](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/fm-overview.html)
- 🤖 [IBM Granite Model Cards](https://www.ibm.com/granite)
- 🐛 [Open an Issue](https://github.com/your-username/course-simplifier/issues)

---

<p align="center">
  Built with ❤️ using <strong>IBM Granite</strong> &amp; <strong>IBM watsonx.ai</strong>
</p>
