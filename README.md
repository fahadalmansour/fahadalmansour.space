# fahadalmansour.space

Static personal site. Everything inside `public/` is the site; nothing else is uploaded.

## Deployment

Pushing to `main` runs `.github/workflows/deploy.yml`, which uploads `public/` to the
cPanel hosting (Namecheap, server `business23.web-hosting.com`) over FTPS using
[SamKirkland/FTP-Deploy-Action](https://github.com/SamKirkland/FTP-Deploy-Action) v4.4.0.
It can also be run manually from the Actions tab (workflow_dispatch).

The action keeps a state file (`.deploy-state.json`) on the server so only changed files
are transferred on later runs.

### Required GitHub Secrets

Set these in GitHub: repository **Settings → Secrets and variables → Actions → New repository secret**.
No credential is stored in this repository.

| Secret | Value |
| --- | --- |
| `FTP_HOST` | `business23.web-hosting.com` (or the FTP host shown in cPanel → FTP Accounts → Configure FTP Client) |
| `FTP_USERNAME` | The cPanel username, or a dedicated FTP account created in cPanel → FTP Accounts |
| `FTP_PASSWORD` | That account's password |
| `FTP_SERVER_DIR` | Target directory on the server, with a trailing slash. For the main domain this is usually `public_html/`. If you create a dedicated FTP account whose home is already `public_html`, use `./` |

Recommendation: create a dedicated FTP account in cPanel whose directory is restricted to
`public_html`, instead of using the main cPanel login. Rotate the password if it is ever exposed.

### First run

The first deploy uploads everything in `public/`. Check the run log in the Actions tab; a
connection or authentication error points to `FTP_HOST`, `FTP_USERNAME` or `FTP_PASSWORD`,
while files landing in the wrong folder points to `FTP_SERVER_DIR`.
