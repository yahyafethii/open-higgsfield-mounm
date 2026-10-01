# Running OpenHiggsfield locally on macOS

## 1. Requirements

| Tool | Version | Install |
| --- | --- | --- |
| Node.js | 20.9+ (22 LTS recommended) | `brew install node@22` or https://nodejs.org |
| pnpm | 10.x (tested with 10.28) | `npm install -g pnpm@10` |
| Git | any | ships with Xcode Command Line Tools (`xcode-select --install`) |

Check with `node -v` and `pnpm -v`.

## 2. Clone and install

```bash
git clone https://github.com/yahyafethii/open-higgsfield-mounm.git
cd open-higgsfield-mounm
pnpm install
```

## 3. Configure

```bash
cp .env.example .env.local
open -e .env.local
```

| Variable | Required for | Where to get it |
| --- | --- | --- |
| `HF_API_BASE_URL` | every generation | Already set to `https://api.higgsfield.ai` in the example. |
| `OPEN_HIGGSFIELD_READ_WRITE_TOKEN` | uploading images/video/audio as inputs | vercel.com → Storage → Create → **Blob** (public access) → the store's `.env.local` tab → copy the `BLOB_READ_WRITE_TOKEN` value (`vercel_blob_rw_…`) into this variable. Free Hobby plan is enough. |

The Higgsfield API key is **not** an environment variable. Create it at
https://cloud.higgsfield.ai (API keys section; requires API credits), then paste it in the
app's **Your key** dialog as `KEY_ID:KEY_SECRET`. It is stored in an httpOnly cookie.

## 4. Run

```bash
pnpm dev
open http://localhost:3000
```

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| `Missing HF_API_BASE_URL` in the terminal, 500 on Generate | `.env.local` missing or empty; set it and restart `pnpm dev`. |
| `API key must be id:secret` | Paste both parts joined by a colon. |
| `Platform request failed (401/403)` | Wrong key or secret, or no API credits on the account. |
| Upload fails, `Missing OPEN_HIGGSFIELD_READ_WRITE_TOKEN` | Add the Vercel Blob token (step 3) and restart. |
| `Port 3000 is in use` | `lsof -ti :3000 \| xargs kill`, or run `pnpm next dev --port 3001`. |
| `ERR_PNPM_…` about build scripts or release age | Upgrade pnpm: `npm install -g pnpm@10`. |
| `Unsupported engine` / syntax errors on start | Node is too old; install Node 22. |
