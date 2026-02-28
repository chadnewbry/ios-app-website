# iOS App Website

A skill that generates and deploys Privacy Policy, Terms of Service, and Support pages for iOS apps using GitHub Pages.

Works with both **Claude Code** and **OpenClaw**.

## Skills

### Claude Code
The skill at `skills/app-legal-pages/SKILL.md` uses Claude Code tools (`AskUserQuestion`, `WebFetch`, `Grep`).

### OpenClaw
The skill at `skills/app-legal-pages/SKILL.openclaw.md` uses OpenClaw tools (`exec`, `read`, `web_fetch`). Install it into your OpenClaw workspace:

```bash
cp -r skills/app-legal-pages ~/.openclaw/workspace/skills/
# Rename for OpenClaw
cd ~/.openclaw/workspace/skills/app-legal-pages
mv SKILL.openclaw.md SKILL.md
```

Or use ClawdHub if published.

## What It Does

1. Scans your iOS codebase to detect third-party services, permissions, data collection, AI features, and subscriptions
2. Generates tailored legal pages from templates
3. Deploys to GitHub Pages
4. Optionally updates in-app URLs

## Default Configuration

- Business: Chad Newbry, LLC
- Contact: chad.newbry@gmail.com
- GitHub: chadnewbry
