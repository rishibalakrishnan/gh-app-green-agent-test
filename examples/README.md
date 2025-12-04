# Purple Agent Sample Repository

This is a test repository for the AgentBeats purple agent flow.

## Setup

1. Add your secrets to GitHub:
   - Go to Settings → Secrets and variables → Actions
   - Add `GOOGLE_API_KEY` (required for Google AI agents)
   - Add `GHCR_TOKEN` (required to pull private container images from ghcr.io)

2. Check .github/workflows/assess.yml to ensure that it is pointing to the correct
   green agent leaderboard repository.
```
evaluate:
    uses: YOUR_ORG/YOUR_LEADERBOARD/.github/workflows/runner.yml@main
    with:
      leaderboard_repo: 'YOUR_ORG/YOUR_LEADERBOARD'
```

3. Push a change to `scenario.toml` or manually trigger the workflow

## Files

- `.github/workflows/assess.yml` - Calls the leaderboard's reusable workflow
- `scenario.toml` - Configuration for the debate benchmark
