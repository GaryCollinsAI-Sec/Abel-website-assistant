# Abel — Website Assistant & Uptime Monitor

Abel is a Chrome extension that combines rule-based website analysis with automated website uptime monitoring. It helps users inspect websites for potential issues, monitor availability, track response times, and review historical monitoring results.

The project combines a React and TypeScript frontend with a Python FastAPI backend.

**Project Status:** Functional Prototype — Version 0.1

## Features

### Website Scanner
- Browser-based website scanning.
- Rule-based detection of potential website issues.
- Findings categorized by severity.
- Suggested troubleshooting and remediation guidance.
- Scan history stored locally in the browser.

### Uptime Monitoring
- Register websites for continuous monitoring.
- Automated availability checks every five minutes.
- Manual website checks.
- HTTP response status detection.
- Response-time measurement.
- Website availability classification:
  - **Up:** Successful HTTP responses.
  - **Degraded:** HTTP client-error responses.
  - **Down:** Server errors, connection failures, and timeouts.
- Historical monitoring records.
- Uptime statistics and average response times.
- Enable, disable, and delete monitored websites.

### Backend Security
- API-key authentication.
- Constant-time API-key comparison.
- Request rate limiting.
- URL validation.
- SSRF protection measures.
- Restricted URL schemes.
- Redirect following disabled.
- CORS configuration.

## Technology Stack

| Component | Technologies |
|---|---|
| Frontend | React, TypeScript |
| Build Tool | Vite |
| Styling | Tailwind CSS |
| Browser Integration | Chrome Extension Manifest V3 |
| Backend | Python, FastAPI |
| HTTP Monitoring | aiohttp |
| Scheduling | APScheduler |
| Database | SQLite |
| Authentication | API-key authentication |
| API Communication | HTTP / JSON |

## Architecture

Abel uses a separated frontend and backend architecture.

```text
Chrome Browser
      |
      v
Abel Chrome Extension
      |
      +----------------------+
      |                      |
      v                      v
Website Scanner        Uptime Monitor UI
      |                      |
      v                      v
Browser Page Analysis   FastAPI Backend
                             |
                +------------+------------+
                |            |            |
                v            v            v
             Monitor      Scheduler    Database
                |            |            |
                v            v            v
             aiohttp     APScheduler    SQLite
```

The website scanner operates independently within the browser extension. The uptime monitoring functionality communicates with the Python backend.

## Project Structure

```text
abel/
├── src/
│   ├── App.tsx
│   ├── UptimePanel.tsx
│   ├── scanner.ts
│   ├── index.css
│   └── main.tsx
│
├── uptime-monitor/
│   ├── app/
│   │   ├── main.py
│   │   ├── monitor.py
│   │   ├── database.py
│   │   ├── scheduler.py
│   │   └── rate_limit.py
│   │
│   ├── data/
│   ├── .venv/
│   └── .env
│
├── icons/
├── manifest.json
├── index.html
├── package.json
├── tsconfig.app.json
├── vite.config.ts
└── README.md
```

*Note: The structure above describes the main project components. Local environment files, databases, and generated build files should not be committed.*

## Local Development Setup

### Requirements

- Python 3.14+
- Node.js and npm
- Google Chrome or a Chromium-based browser

### 1. Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
cd abel
```

Replace `YOUR_REPOSITORY_URL` with your repository's clone URL.

### 2. Install Frontend Dependencies

```bash
npm install
```

### 3. Configure the Python Backend

```bash
cd uptime-monitor

python3 -m venv .venv

source .venv/bin/activate

pip install fastapi uvicorn aiohttp apscheduler pydantic python-dotenv
```

### 4. Configure Environment Variables

Create a `.env` file inside `uptime-monitor/`.

```env
ABEL_API_KEY=your_generated_secret_key
```

Generate a secure key locally rather than using the example value:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Never commit your `.env` file or expose your API key.

### 5. Start the Backend

From the `uptime-monitor/` directory:

```bash
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

The backend will be available at:

```text
http://127.0.0.1:8000
```

API documentation:

```text
http://127.0.0.1:8000/docs
```

### 6. Build the Chrome Extension

Open a second terminal:

```bash
cd abel
npm run build
```

The compiled extension will be generated in the `dist/` directory.

### 7. Load Abel into Chrome

1. Open `chrome://extensions`.
2. Enable Developer Mode.
3. Select **Load unpacked**.
4. Select the project's `dist/` directory.
5. Open the Abel extension.
6. Enter your locally generated API key in the Uptime Monitor configuration.

The backend must be running for uptime monitoring to work.

## API Overview

The FastAPI backend exposes the following endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/health` | Backend health check |
| GET | `/api/health` | API health check |
| GET | `/api/scheduler` | Scheduler status |
| POST | `/api/websites` | Register a website |
| GET | `/api/websites` | List monitored websites |
| GET | `/api/websites/{id}` | Retrieve website details |
| PATCH | `/api/websites/{id}` | Enable or disable monitoring |
| DELETE | `/api/websites/{id}` | Delete a monitored website |
| POST | `/api/websites/{id}/check` | Run a manual check |
| GET | `/api/websites/{id}/history` | Retrieve monitoring history |
| GET | `/api/websites/{id}/statistics` | Retrieve uptime statistics |

Protected endpoints require API-key authentication.

## Testing and Verification

The following functionality has been tested during local development:

- Frontend TypeScript compilation and production build.
- Backend Python syntax validation.
- API-key authentication.
- Protected endpoint access.
- CORS configuration.
- Request rate limiting.
- Website registration.
- Manual availability checks.
- Automatic scheduled monitoring.
- Monitoring history persistence.
- Uptime statistics retrieval.
- Unreachable website detection.
- URL validation and SSRF protection checks.

## Security Considerations

Abel includes several security controls, but this project is currently a local functional prototype.

Important considerations before production deployment:

- Replace the development API-key model with a proper multi-user authentication system.
- Use a shared rate-limiting store when deploying multiple backend workers.
- Perform additional SSRF and DNS-rebinding security testing.
- Configure HTTPS for remote deployments.
- Establish secure secret management.
- Add production monitoring, logging, and operational safeguards.
- Review CORS and network access policies for the deployment environment.

The current local API key is intended for development and should not be treated as a production authentication architecture.

## Development Philosophy

Abel is designed around conventional programming and deterministic automation.

The project uses explicit rules, application logic, HTTP requests, and scheduled tasks rather than AI-generated analysis.

**Abel does not use AI APIs, language models, or autonomous AI agents.**

## Current Development Status

**Version 0.1 — Functional Prototype**

The initial prototype successfully integrates the existing website scanner with a Python-powered uptime monitoring service.

Future development may include:

- Improved scanner detection rules.
- Enhanced monitoring dashboards.
- Historical uptime reporting.
- Notification integrations.
- Production deployment.
- Multi-user account support.

## License

A license has not yet been assigned. All rights are reserved by the project owner unless a license is added to this repository.
