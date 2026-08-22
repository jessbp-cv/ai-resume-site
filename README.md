# Jessica Pelegio — AI Resume Site

A static site with a live AI chat, grounded in the career record in `api/chat.js`.
The Anthropic API key stays server-side only — it is never sent to the browser.

## Files

- `index.html` — the whole site. Calls `/api/chat` for the chat feature.
- `api/chat.js` — serverless function. Holds the resume text, the system
  prompt, and calls Anthropic using a key read from an environment variable.
  Also does basic rate limiting.

## Deploy to Vercel (recommended, free tier is enough)

1. **Create a GitHub repo and push this folder:**

   ```bash
   cd ai-resume-site
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```

   (If you'd rather skip git entirely: create a new repo on github.com, then
   use the "uploading an existing file" link on the repo page to drag and
   drop `index.html`, the `api` folder, and `.gitignore`.)

2. **Import into Vercel:**
   - Go to vercel.com → sign in with GitHub → "New Project" → pick this repo.
   - Leave build settings as default (it's a static site + one function —
     Vercel detects both automatically). Click Deploy.

3. **Add your API key as an environment variable (do this in Vercel, never in code):**
   - In the project → Settings → Environment Variables.
   - Name: `ANTHROPIC_API_KEY`
   - Value: your key from console.anthropic.com
   - Save, then go to Deployments and redeploy so the function picks it up.

4. **Test it:**
   - Vercel gives you a free URL like `your-project.vercel.app`.
   - Open it, try the chat. Check Vercel's function logs if anything errors.

5. **Attach your own domain (optional):**
   - Project → Settings → Domains → add your domain.
   - Vercel shows you the DNS records to add at your registrar (usually an
     A record or CNAME). Propagation is often near-instant, sometimes a few hours.

## Updating the content later

Edit the `RESUME_CONTEXT` and `SYSTEM_PROMPT` constants at the top of
`api/chat.js`, commit, and push — Vercel redeploys automatically on every
push to `main`.

## Cost and safety notes

- Anthropic API usage is billed per token to whatever account owns the key.
  Set a budget alert in console.anthropic.com so you're not surprised.
- `api/chat.js` includes a simple per-IP rate limit (8 requests/minute) to
  blunt casual abuse. It resets on cold start, so it's a speed bump, not a
  hard guarantee — fine for a personal site's traffic level.
- Never commit an API key to the repo or paste it in chat/email. It only
  ever belongs in Vercel's Environment Variables.
