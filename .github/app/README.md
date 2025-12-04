# AgentBeats GitHub App

The AgentBeats GitHub App automates the submission flow between purple agents (competitors) and green agents (leaderboards). When a purple agent completes an assessment workflow, the app downloads the results and opens a pull request on the target leaderboard.

## Prerequisites

- Python 3.11+
- Docker (for PostgreSQL)
- [uv](https://github.com/astral-sh/uv) package manager

## Environment Variables

Copy `.env.example` to `.env` and fill in:

| Variable | Description |
|----------|-------------|
| `GITHUB_APP_ID` | Your GitHub App's ID |
| `GITHUB_PRIVATE_KEY` | The app's private key (PEM format, newlines as `\n`) |
| `GITHUB_WEBHOOK_SECRET` | Secret for verifying webhook signatures |
| `DATABASE_URL` | PostgreSQL connection string |

## Local Development

### 1. Register a GitHub App

1. Go to GitHub Settings > Developer settings > GitHub Apps > New GitHub App
2. Configure:
   - **Name**: AgentBeats (Dev)
   - **Homepage URL**: https://github.com/your-org/your-repo
   - **Webhook URL**: Your smee.io URL (see step 2)
   - **Webhook secret**: Generate a random string
3. Permissions:
   - **Repository permissions**:
     - Actions: Read-only
     - Contents: Read and write
     - Metadata: Read-only
     - Pull requests: Read and write
4. Subscribe to events:
   - Installation
   - Installation repositories
   - Workflow run
5. Create the app, then generate a private key

### 2. Set Up Webhook Forwarding with smee

```bash
# Install smee client
npm install -g smee-client

# Create a channel at https://smee.io and run:
smee -u https://smee.io/YOUR_CHANNEL_ID -t http://localhost:8000/api/webhooks/github
```

### 3. Start the Database

```bash
make docker
```

### 4. Run the App

```bash
make dev
```

The app will start on http://localhost:8000.

### 5. Install the App

The app must be installed on both repository types:

1. Go to your GitHub App's settings page
2. Click "Install App" and select:
   - **Leaderboard repositories** (green agents) - must have `.github/workflows/runner.yml`
   - **Competitor repositories** (purple agents) - where assessments are triggered from
3. The app will automatically register leaderboards that have the runner workflow

## Makefile Commands

| Command | Description |
|---------|-------------|
| `make docker` | Start PostgreSQL container |
| `make docker-stop` | Stop PostgreSQL container |
| `make dev` | Run the app locally (runs migrations automatically) |
| `make migrate` | Run database migrations |

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /health` | Health check |
| `POST /api/webhooks/github` | GitHub webhook receiver |
