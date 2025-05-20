# Dependency Bot

A GitHub App that scans pull requests for dependency license and risk issues, then comments on the PR with detailed findings.

## Code Review Summary

- **Modular Design**: Code is organized into logical packages (`depsdev`, `parsers`, `utils`), making it easy to extend support for additional ecosystems.
- **Authentication & Security**: Uses JWT-based authentication (`auth.py`) and signature verification (`utils/signature_verifier.py`) to securely handle GitHub webhooks and API requests.
- **Dockerization**: Includes a `Dockerfile` and `docker-compose.yml` for containerized development and deployment.
- **Areas for Improvement**:
  - Centralize configuration: consider using a `config.py` or environment-loading library to validate and manage all environment variables in one place.
  - Logging: replace print statements (if present) with the `logging` module to enable configurable log levels and better troubleshooting.
  - Error Handling: add more granular exception handling around API calls and parsing routines to avoid crashing on unexpected input.
  - Testing: include unit tests for parsers and utility functions to ensure reliability when adding new features.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running Locally](#running-locally)
- [Docker](#docker)
- [GitHub App Setup](#github-app-setup)
- [Usage](#usage)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

- Python 3.9 or higher
- Docker & Docker Compose (if using containers)
- GitHub account with permissions to create a GitHub App

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-org/dependency-bot.git
   cd dependency-bot
   ```
2. Create a virtual environment and install dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

## Configuration

The application requires several environment variables:

| Variable                 | Description                                  |
| ------------------------ | -------------------------------------------- |
| `GITHUB_APP_ID`          | Your GitHub App ID                           |
| `GITHUB_APP_PRIVATE_KEY` | Path to the GitHub App private key (.pem)    |
| `WEBHOOK_SECRET`         | Secret used to secure webhook payloads       |
| `FLASK_ENV`              | (optional) `development` or `production`     |

You can set these in a `.env` file at the project root:

```dotenv
GITHUB_APP_ID=12345
GITHUB_APP_PRIVATE_KEY=./keys/app_private_key.pem
WEBHOOK_SECRET=your_webhook_secret
FLASK_ENV=development
```  

## Running Locally

```bash
source venv/bin/activate
export $(grep -v '^#' .env | xargs)
python app.py
```  
The server will listen on port 5000 by default.

## Docker

Build and run with Docker Compose:

```bash
docker-compose build
docker-compose up
```

## GitHub App Setup

1. **Create a new GitHub App:**
   - Go to **Settings > Developer settings > GitHub Apps** and click **New GitHub App**.
   - **Name:** Dependency Bot
   - **Homepage URL:** `https://your-domain.com`
   - **Webhook URL:** `https://your-domain.com/webhook`
   - **Webhook secret:** (copy into `WEBHOOK_SECRET`)

2. **Permissions & Events:**
   - **Repository permissions:**
     - **Contents:** Read-only
     - **Pull requests:** Read & write
     - **Issues:** Read & write
   - **Subscribe to events:**
     - **Pull request**
     - **Pull request review**
     - **Pull request review comment**

3. **Generate a private key:**
   - After creating the app, generate and download the private key.
   - Save it (e.g., `keys/app_private_key.pem`) and reference its path in `GITHUB_APP_PRIVATE_KEY`.

4. **Install the App on your repository:**
   - Go to your GitHub App settings and click **Install App**.
   - Choose the repository (or organization-wide) installation.

## Usage

Once the app is installed and running, it will automatically scan incoming pull requests and post comments with:

- License compliance details
- Risk classification of dependencies
- Suggested remediation or further reading links

## Directory Structure

```
├── app.py                # Flask webhook entry point
├── auth.py               # JWT authentication helper
├── depsdev/              # Integrations with external dependency data sources
│   ├── maven.py
│   ├── npm.py
│   └── pypi.py
├── parsers/              # Dependency manifest parsers
│   ├── maven_parser.py
│   ├── node_parser.py
│   └── python_parser.py
├── utils/                # Core processing and commenting logic
│   ├── pr_processor.py
│   ├── pr_commenter.py
│   ├── risk_classifier.py
│   ├── risky_issue_creator.py
│   └── signature_verifier.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── .github/
    └── workflows/deploy.yml
```

## Contributing

Contributions are welcome! Please open an issue or pull request for any improvements, bug fixes, or new features.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
