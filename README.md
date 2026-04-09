# CodeHunt

Search real GitHub code with one-click GitHub login — deployable to Netlify in minutes.

## How the auth works

```
User clicks "Sign in" → GitHub OAuth page
→ GitHub redirects to your-site.netlify.app/?code=abc
→ Netlify function /api/auth exchanges code for token (secret stays server-side)
→ Token stored in sessionStorage, used for all searches
→ Searches go through /api/search (serverless function, proxies GitHub API)
```

---

## Deploy to Netlify

### 1. Create a GitHub OAuth App

Go to **github.com → Settings → Developer settings → OAuth Apps → New OAuth App**

| Field | Value |
|-------|-------|
| Application name | CodeHunt |
| Homepage URL | `https://your-site.netlify.app` |
| Authorization callback URL | `https://your-site.netlify.app` |

> The callback URL is your Netlify site root — the frontend handles the `?code=` param.

Copy the **Client ID** and generate a **Client Secret**.

---

### 2. Deploy

```bash
# Push this folder to a GitHub repo, then:
# Go to netlify.com → Add new site → Import from GitHub → select your repo
# Netlify auto-detects vite + builds it
```

### 3. Set environment variables in Netlify

Go to **Site → Site configuration → Environment variables** and add:

| Key | Value |
|-----|-------|
| `GITHUB_CLIENT_ID` | your client id |
| `GITHUB_CLIENT_SECRET` | your client secret |
| `VITE_GITHUB_CLIENT_ID` | your client id (same value) |

Redeploy after adding vars.

---

## Local development

```bash
cp .env.example .env
# Fill in your GitHub OAuth credentials

npm install
npm run dev
# Opens at http://localhost:8888
# Netlify CLI runs both Vite + serverless functions together
```

For local OAuth, set the GitHub app callback URL to `http://localhost:8888` temporarily.
