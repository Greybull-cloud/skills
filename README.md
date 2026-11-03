# Greybull skills

Skills for AI coding assistants (Claude Code, OpenAI Codex and others) that work with
[Greybull](https://greybull.cloud), Dutch hosting.

| Skill | What it does |
|---|---|
| [`greybull-publish`](skills/greybull-publish/SKILL.md) | Publish a website you built or designed with AI. Say "publish my site" and it goes live at `https://<site>-<account>.greybull.site`. |

You need a Greybull account with AI publishing enabled (early access:
[request access](https://greybull.cloud/contact)). The skill uses the
[`greybull` CLI](https://www.npmjs.com/package/greybull) (Node 18+ and git).

## Install

**Claude Code**

```
/plugin marketplace add Greybull-cloud/skills
/plugin install greybull@greybull
```

**OpenAI Codex, Cursor and other agents**

```
npx skills add Greybull-cloud/skills
```

Or copy `skills/greybull-publish` into your agent's skills folder (Codex: `~/.agents/skills/`).

## Use

Open your site's folder in your assistant and ask it to publish the site. The first time, your
browser opens to log in to Greybull, and you choose a plan for the new site. After that, ask it
to publish again whenever you change something.

More: <https://greybull.cloud/for/ai-builders>
