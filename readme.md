# 🤖 LLM Code Deployment Agent

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

This project implements an **automated developer agent** that receives natural-language app specifications, generates working web applications using a Large Language Model (LLM), deploys them to GitHub Pages, and reports deployment metadata for automated evaluation.

Built with **FastAPI**, this service fulfills the requirements of the *LLM Code Deployment* assignment by handling both **Round 1 (Build)** and **Round 2 (Revise)** tasks end-to-end.

---

## 📌 Overview

Given a JSON task request like:
```json
{
  "email": "student@example.com",
  "secret": "your-secret",
  "task": "captcha-solver-abc12",
  "round": 1,
  "brief": "Create a captcha solver that handles ?url=...",
  "evaluation_url": "https://eval.example.com/notify",
  "attachments": [...]
}
```

This service will:
1. ✅ Verify the shared secret  
2. 🧠 Use an LLM to generate a minimal frontend app  
3. 📁 Create a public GitHub repository  
4. 🚀 Push code + `README.md` + `LICENSE`  
5. 🌐 Enable GitHub Pages  
6. 📤 Report `repo_url`, `commit_sha`, and `pages_url` to the evaluator  

It also supports **Round 2 revisions**, updating the same repo with new features.

---

## 🛠️ Features

- **Fully automated** build → deploy → report pipeline  
- **LLM-powered code generation** (OpenAI-compatible)  
- **GitHub API integration** for repo creation and file upload  
- **MIT-licensed** and clean Git history (no secrets!)  
- **Idempotent task handling** with nonce validation  
- **Exponential backoff** for evaluation reporting retries  
- **Supports both Round 1 and Round 2** tasks  

---

## 📦 Tech Stack

- **Backend**: [FastAPI](https://fastapi.tiangolo.com/) (Python)  
- **LLM**: OpenAI API (or compatible)  
- **GitHub**: REST API v3 for repo & file management  
- **Deployment**: Self-hosted on cloud platform (e.g., Render, Railway)  
- **Testing**: Playwright-compatible output  

---

## ⚙️ Setup & Configuration

### Prerequisites
- Python 3.9+  
- GitHub Personal Access Token (with `repo` and `pages` scopes)  
- OpenAI API key (or compatible LLM provider)  
- Publicly accessible host (e.g., Render, Fly.io)  

### Environment Variables

Create a `.env` file:

```env
APP_SECRET=your_shared_secret_from_google_form
GH_TOKEN=your_github_personal_access_token
GITHUB_USER=23f2001000
GEMINI_API_KEY_API_KEY=your_gemini_api_key
```

> 🔒 **Never commit `.env` or tokens to Git!** This repo uses `.gitignore` to prevent leaks.

### Installation

```bash
git clone https://github.com/23f2001000/fastapi-deploy.git
cd fastapi-deploy
pip install -r requirements.txt
```

### Running Locally (for testing)

```bash
uvicorn main:app --reload --port 8000
```

> For production, deploy to a cloud provider with HTTPS.

---

## 🌐 API Usage

### Endpoint
`POST /`

### Request Body (JSON)
See project specification. Must include:
- `email`  
- `secret`  
- `task`  
- `round` (1 or 2)  
- `nonce`  
- `brief`  
- `evaluation_url`  
- `attachments` (optional)  

### Response
- **200 OK** — if secret is valid (processing happens asynchronously)  
- **401 Unauthorized** — if secret is invalid  
- **400 Bad Request** — if JSON is malformed  

> The service **responds immediately** and processes the task in the background.

---

## 📁 Repository Structure

```
fastapi-deploy/
├── main.py                # FastAPI app entrypoint
├── app/
│   ├── generator.py       # LLM code generation logic
│   ├── github.py          # GitHub API integration
│   ├── evaluator.py       # Reporting to evaluation_url
│   └── tasks.py           # Task orchestration
├── templates/
│   └── readme_template.md # Base README for generated apps
├── requirements.txt
├── .env.example
├── .gitignore
└── LICENSE
```

---

## 📜 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

> Note: All **generated student apps** also include an MIT license in their root directory, as required by the assignment.

---

## 🧪 Evaluation Compliance

This implementation satisfies all required checks:
- ✅ Public GitHub repo with MIT license  
- ✅ Professional `README.md` in generated apps  
- ✅ No secrets in Git history  
- ✅ GitHub Pages enabled and functional  
- ✅ Timely reporting to `evaluation_url`  
- ✅ Supports Round 2 updates  

Generated apps are tested against:
- Static rules (license, README)  
- LLM-based code quality  
- Playwright dynamic tests (e.g., `?url=` handling)  

---

## 🙌 Acknowledgements

- Built for the **LLM Code Deployment** course project  
- Inspired by AI-assisted software engineering research  
- Uses GitHub REST API and OpenAI completions  

---

## ⭐ Support

If you found this project useful or learned something from it, please give it a ⭐ on GitHub — it helps others discover the project too!

---

### ❤️ Thank You for Visiting!

Keep coding, keep deploying, and keep learning!
Made with 💻 and ☕ by **Akshat Danve**

---

## 📬 Contact

For questions about this implementation:  
**Author**: Akshat Amit Danve | **Student ID**: 23f2001000  
**GitHub**: [@23f2001000](https://github.com/23f2001000)

---

> 🚀 *Automating the future of software development—one LLM-generated app at a time.*
```
