# Early License Risk Detection Bot

**JPMC Hackathon 2025 Submission**
**Track:** Open-Source Governance Automation

## 🏁 Project Overview
Early License Risk Detection Bot (`osg-dependency-bot`) empowers developers by scanning dependencies at pull request time, classifying license risk, and automating governance workflows before code merges into production.

**Hackathon Challenge Alignment:**
- Embeds compliance checks into developer workflow (DevOps integration)  
- Demonstrates event-driven, scalable architecture  
- Lays groundwork for AI-driven self-healing pipelines (future Innovation Track)

---

## 🎯 Objectives
1. **Proactive Governance:** Detect Red/Yellow license risks early in PRs.  
2. **Developer Empowerment:** Provide actionable feedback directly in GitHub PR comments.  
3. **Seamless Automation:** Automatically open GitHub Issues for any risky licenses merged.

---

## ✅ Prototype Status
- **Webhooks & JWT Auth:** Secure handling of GitHub events (`auth.py`).  
- **Dependency Parsing:** Supports Maven manifests (`pom.xml`) via `parsers/maven_parser.py`.  
- **Risk Classification:** Leverages DepsDev API to tag licenses as Safe, Risky, or High Risk.  
- **Automated Comments & Issues:** Posts detailed reports on PRs and creates Issues post-merge.  
- **Containerized Deployment:** `Dockerfile` & `docker-compose.yml` for local or cloud hosting.

---

## 🚀 Quick Start

### Prerequisites
- Python 3.9+  
- Docker & Docker Compose  
- GitHub account with App creation permissions

### 1. Clone Repository
```bash
git clone https://github.com/your-org/osg-dependency-bot.git
cd osg-dependency-bot
```

### 2. Install Dependencies & Setup
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your values
```  

| Variable                 | Description                                 |
|--------------------------|---------------------------------------------|
| `GITHUB_APP_ID`          | GitHub App ID                               |
| `GITHUB_APP_PRIVATE_KEY` | Path to PEM private key                     |
| `WEBHOOK_SECRET`         | Webhook secret for payload verification     |
| `FLASK_ENV`              | `development` or `production`               |

### 3. Run Locally
```bash
export $(grep -v '^#' .env | xargs)
python app.py
```  
Server listens on **port 5000** by default.

### 4. Docker Deployment
```bash
docker-compose build
docker-compose up -d
```

---

## 🔧 GitHub App Setup
1. **Register App:**  
   - Settings → Developer settings → GitHub Apps → New GitHub App  
   - Name: **Dependency Risk Detection Bot**  
   - Homepage URL: `https://demo.your-domain.com`  
   - Webhook URL: `https://demo.your-domain.com/webhook`  
   - Webhook Secret: copy into `.env`
2. **Permissions:**  
   - Repositories → Contents: **Read-only**  
   - Pull requests: **Read & write**  
   - Issues: **Read & write**  
3. **Events Subscribed:**  
   - Pull request  
   - Pull request review  
   - Pull request review comment
4. **Generate & Download Private Key:**  
   - Save as `keys/app_private_key.pem` and update `.env`
5. **Install App:**  
   - Install on target repo or organization-wide

---

## 🗂️ Architecture
```text
GitHub PR event → Flask Webhook (app.py)
  └─> Signature Verification → Dependency Parser → Risk Classifier → Commenter
  └─> On merge: Issue Creator → (Future) FARM Findings Integration
```

---

## 📸 Demo

Try the bot live on our sample Pull Request: [View Sample PR](https://github.com/your-org/osg-dependency-bot/pull/1)

![Dependency Scan Report – Collapsed View](docs/screenshots/dependency-report-collapsed.PNG)
*Collapsed summary showing license risk legend and sections.*

![Dependency Scan Report – Expanded Table](docs/screenshots/dependency-report-expanded.PNG)
*Detailed table listing scanned dependencies, versions, licenses, risk classifications, and sources.*

---

---

## 📭 Issue Alerts

When a PR with risky/high-risk licenses is merged, the bot automatically opens a GitHub Issue for governance tracking:

![License Risk Alert Issue](docs/screenshots/license-risk-alert-issue.PNG)
*Automated issue created showing detected high-risk dependencies and prompting for remediation.*

---

## 🔮 Next Steps
- **AMS API Integration:** Replace DepsDev with internal license classification.  
- **FARM Findings Sync:** Auto-create records in governance platform.  
- **Gen AI Build Failure Assistant:** Extend to CI/CD pipeline self-healing (innovation track).  

---

## 🙋‍♂️ Team & Contact
**Satish Paladugu** (Project Lead)  
Email: satish.paladugu@yourfirm.com

Thank you for reviewing our submission—looking forward to feedback!

