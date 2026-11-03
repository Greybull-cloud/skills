---
name: greybull-publish
description: Publish a static website (HTML/CSS/JS, or a site with an npm build step such as Vite, Astro or a Next.js static export) to the internet with Greybull, Dutch hosting. Use when the user asks to publish, deploy, host, put online, go live with or share a website they built or designed, including sites exported from ChatGPT, Claude or Claude Design, and to publish updates to a site published before.
---

# Publish a website with Greybull

Greybull hosts the site in the Netherlands at `https://<site>-<account>.greybull.site`. You do
the technical part; the user logs in once and chooses a plan once per site.

Everything goes through the `greybull` command line tool (`npx -y greybull@latest`, needs
Node 18+ and git). Add `--json` to get machine-readable output on stdout.

## 1. Get the site ready

Greybull serves **static** websites: files a browser loads directly.

- The folder needs an `index.html` at the top, **or** a `package.json` with a `build` script
  whose output (`dist`, `build`, `out`, `_site`, `public` or `.output/public`) has an
  `index.html`. Greybull runs the build; you do not need to build first.
- A site exported or downloaded from ChatGPT, Claude or Claude Design (loose files or a zip):
  unzip it into its own folder with `index.html` at the top, not in a subfolder. Fix links to
  `localhost` or to files that are not in the folder.
- `about.html` and `about/index.html` are both served at `/about`; a top-level `404.html` is
  shown for missing pages.
- Not supported yet: anything that needs a server while running (API routes, server-side
  rendering, a database, PHP, server-side form handling). Tell the user this kind of site can't
  be published with Greybull yet instead of publishing something that won't work.
- Everything in the site is public: never put API keys, passwords or `.env` contents in it.

## 2. Publish

From the site folder (or pass the folder as an argument):

```
npx -y greybull@latest --json publish
```

- Not logged in yet: the command opens the user's browser to log in to Greybull and approve
  access, and continues once they have. Tell the user to approve it in the browser. If you
  can't open a browser, ask the user to run `npx greybull login --scopes services:read,services:write`
  themselves. Never ask them to paste an API key or password into the chat.
- The site is named after the folder; `--name <name>` picks another (lowercase letters, digits,
  dashes). The folder is committed with git and pushed to Greybull; this is remembered, so
  later publishes from the same folder update the same site.
- It waits while Greybull builds and deploys (usually one to two minutes).

Exit codes: `0` live · `6` waiting for the user to choose a plan · `3` login or access problem
(the message says what to do) · `2` the folder can't be published (the message says why) ·
anything else: failed, with the reason.

### A new site: the user chooses the plan

A site's first publish stops with exit code **6** and `"status": "awaiting_plan"`, a `summary`
of what was found, a `recommended` size and the `options` with their monthly price in euros,
excluding VAT. Hosting is paid monthly for the chosen plan.

**Never choose the plan yourself, not even the recommended one.** Show the user the summary
and the options briefly (size, price per month, which one is recommended), ask which one they
want, and only after they answer run:

```
npx -y greybull@latest --json sites plan <site> <size>
```

This starts the plan and waits until the site is live.

## 3. Tell the user

On success (`"action": "published"`), give the user the URL. Say you can publish changes the
same way whenever they want.

If publishing failed, the error says why (for a failed build, its last output). Fix what you
can in the site and publish again; otherwise explain the problem in plain words.

- "AI publishing is not enabled for this Greybull account": the account isn't in the early
  access yet. Point the user to https://greybull.cloud/contact and stop.

## Updates and status

- Changes: run `npx -y greybull@latest --json publish` again from the same folder. No plan
  question; the site keeps its plan. Every publish is a new version.
- `npx -y greybull@latest sites list` shows the user's sites; `sites status [site]` shows a
  site's address and its recent publishes.
