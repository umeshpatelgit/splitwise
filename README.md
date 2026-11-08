# SplitRupee ( GitHub Pages )

Splitwise-style expense splitting in ₹ on GitHub Pages. Each group is saved as `data/<group-name>.json` in a separate private data repo through the GitHub API.

Group link: `https://<you>.github.io/<repo>/?g=goa-trip`

## Setup
1. **Data repo:** create a private repo (e.g. `splitrupee-data`) with a README so it has a `main` branch.
2. **Token:** Settings → Developer settings → Fine-grained tokens → only the data repo → Contents: Read and write.
3. **App repo:** push this folder (`index.html` at the root plus `.github/workflows/pages.yml`) to `main`.
4. App repo → Settings → Secrets and variables → Actions:
   - `DATA_REPO` (variable or secret) = `owner/splitrupee-data`
   - `DATA_TOKEN` (secret) = your token
5. Settings → Pages → Source: **GitHub Actions**.
6. Actions tab → run "Deploy to GitHub Pages" and wait for the green tick.

The workflow writes `config.js` at deploy time, so the token is never committed. Don't commit a `config.js` with a real token.

## Notes
- The token ends up in the deployed JavaScript. It only reaches the data repo, but anyone viewing the source can read or edit every group.
- If the token expires, update the `DATA_TOKEN` secret and re-run the workflow.
