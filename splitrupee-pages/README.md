# SplitRupee (GitHub Pages edition)

Splitwise-style expense splitting in ₹. The site is static and runs on GitHub Pages.
Each group is saved as `data/<group-name>.json` in a separate **data repo**, written through the GitHub API.

Group link: `https://<you>.github.io/<repo>/?g=goa-trip`

## One-time setup

### 1. Data repo (stores the JSON)
1. Create a new **private** repo, e.g. `splitrupee-data`, with "Add a README" ticked so it has a `main` branch.
2. Go to Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate.
   - Repository access: **Only select repositories** → `splitrupee-data`
   - Permissions → Repository → **Contents: Read and write**
   - Set the longest expiry you're comfortable with, and copy the token.

### 2. App repo (this code)
1. Create a repo, e.g. `splitrupee`, and push this folder to `main`.
2. Settings → Secrets and variables → Actions → New repository secret:
   - `DATA_REPO` = `your-username/splitrupee-data`
   - `DATA_TOKEN` = the token from step 1
3. Settings → Pages → Source: **GitHub Actions**.
4. Open the Actions tab and run "Deploy to GitHub Pages" (or push a commit).

Your site is live at `https://<you>.github.io/splitrupee/`.

## Things to know
- The token is placed in the deployed JavaScript so every visitor's browser can save data. It can only touch the data repo, but anyone who views the site source can use it to read or edit every group. Treat group links as "anyone who has it can edit", and don't use this for sensitive money data. To hide the token, put a small Cloudflare Worker in front of the GitHub API.
- If the token expires, saving stops until you create a new one and update the `DATA_TOKEN` secret.
- Simultaneous saves are handled by retrying, but heavy simultaneous use will be slow because every change is a Git commit.
- Never commit your token into `config.js`; GitHub auto-revokes tokens found in public repos.
