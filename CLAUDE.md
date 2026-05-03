# hello-weather

## Git workflow

After completing any task, always:

1. Commit changes to the feature branch (`claude/...`)
2. Push using the PAT stored in `GITHUB_PAT` env var:
   ```bash
   git remote set-url origin https://${GITHUB_PAT}@github.com/markburns95/hello-weather.git
   git push -u origin <branch>
   ```
3. Open a PR against `main` via the GitHub API:
   ```bash
   curl -s -X POST \
     -H "Authorization: token ${GITHUB_PAT}" \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/repos/markburns95/hello-weather/pulls \
     -d "{\"title\":\"<title>\",\"head\":\"<branch>\",\"base\":\"main\",\"body\":\"<body>\"}"
   ```
4. Immediately merge the PR:
   ```bash
   curl -s -X PUT \
     -H "Authorization: token ${GITHUB_PAT}" \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/repos/markburns95/hello-weather/pulls/<number>/merge \
     -d '{"merge_method":"merge"}'
   ```

The GitHub MCP tools (`mcp__github__*`) do NOT have write access — always use the PAT via curl or git directly.

`GITHUB_PAT` is set in `.claude/settings.local.json` (gitignored) and available as an env var each session.

## Weather page routine

Each day: pick a random year (1995–2009), generate `sandy-weather-YYYY-MM-DD.html` styled in that era's web design fads, with hourly temps and precipitation for Sandy, UT (84047) from 5am–10am. Message @Mark Burns on Slack (user ID: `UJVTWEZ3J`) with the hourly data and a hint about the design style.
