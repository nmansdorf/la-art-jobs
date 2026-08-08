# Getting this live — Windows

You're on `nickpc`, so these are PowerShell commands. Everything below runs from this folder.

## 0. Check you have the tools

```powershell
git --version
gh --version
```

If `git` is missing: https://git-scm.com/download/win
If `gh` is missing: `winget install --id GitHub.cli`

## 1. Authenticate (one time)

```powershell
gh auth login
```

Choose **GitHub.com** → **HTTPS** → **Login with a web browser**, and paste the one-time code it gives you. Do this yourself — I deliberately don't touch credentials.

## 2. Create the repo and push

From inside this folder:

```powershell
cd $HOME\Downloads\la-art-jobs

git init -b main
git add -A
git commit -m "LA concept art job search site"

gh repo create la-art-jobs --public --source=. --remote=origin --push
```

That creates the repo under your account and pushes `main` in one step.

## 3. Turn on Pages

One command, no clicking:

```powershell
gh api -X POST repos/:owner/la-art-jobs/pages -f "source[branch]=main" -f "source[path]=/"
```

Or in the browser if you prefer: `gh repo view --web` → **Settings** → **Pages** → **Source: Deploy from a branch** → `main` / `/ (root)` → Save.

There's no build step and no workflow file — GitHub serves the files straight from the branch. The `.nojekyll` file tells it not to run Jekyll over them.

## 4. Your URL

```
https://<your-github-username>.github.io/la-art-jobs/
```

First build takes a minute or two. Once it's green, update the link at the top of `README.md`.

---

## Updating it later

Edit the files, then:

```powershell
git add -A
git commit -m "Refresh listings"
git push
```

Pages redeploys automatically.

## Where the data lives

All twenty job entries are one JavaScript array at the bottom of `jobs.html` — search for `const JOBS = [`. Add, edit or delete entries there and the stat tiles, filters and grouping all update themselves. Nothing else needs touching.

The networking contacts in `network.html` are plain HTML tables. Edit them directly.

## If something breaks

- **404 for a minute or two** — normal on first deploy. Give it five minutes before worrying.
- **404 that persists** — check Settings → Pages actually shows `main` / `/ (root)` as the source.
- **Pages tab missing** — the repo needs to be public, or you need GitHub Pro for private-repo Pages.
- **Site loads but looks unstyled** — shouldn't happen; all CSS is inline. If it does, hard-refresh (Ctrl+F5).

## One thing to decide

This repo is public, so the site is indexable by search engines. Everything on it is public professional information already, but `network.html` names about forty working art directors alongside a job search, and it's reasonable to not want that surfacing in a search for their names.

If you'd rather it stay link-only, add this line inside the `<head>` of all three HTML files:

```html
<meta name="robots" content="noindex, nofollow">
```

The site still works exactly the same; search engines just skip it.
