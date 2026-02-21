# App Legal Pages — Claude Code Skill

A Claude Code skill that generates and deploys Privacy Policy, Terms of Service, and Support pages for iOS apps using GitHub Pages.

## What it does

When triggered, this skill will:

1. **Scan your iOS codebase** to detect third-party services, device permissions, data collection, AI features, and subscriptions
2. **Ask you** for business name, contact email, and confirmation of detected info
3. **Generate** Privacy Policy, Terms of Service, and Support pages tailored to your app
4. **Deploy** them to a GitHub Pages site
5. **Optionally update** in-app URLs in your source code

## Installation

Add this skill to your Claude Code configuration. You can install it globally or per-project.

### Global install (available in all projects)

Add to `~/.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/ios-app-website/skills/app-legal-pages"
  ]
}
```

### Per-project install

Add to your project's `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/ios-app-website/skills/app-legal-pages"
  ]
}
```

### From GitHub

Clone the repo and reference the local path:

```bash
git clone https://github.com/chadnewbry/ios-app-website.git ~/.claude/skills/ios-app-website
```

Then add to settings:

```json
{
  "skills": [
    "~/.claude/skills/ios-app-website/skills/app-legal-pages"
  ]
}
```

## Usage

In Claude Code, use any of these:

```
/app-legal-pages
```

Or just ask naturally:
- "Create a privacy policy for my app"
- "I need legal pages for App Store submission"
- "Generate terms of service and privacy policy"
- "Set up a support page for my iOS app"

## What gets generated

| Page | Description |
|------|-------------|
| `privacy.md` | Privacy Policy covering data collection, third-party services, permissions, and user rights |
| `terms.md` | Terms of Service with subscription terms, AI disclaimers, and UGC policies (as applicable) |
| `support.md` | Support page with contact info and FAQ tailored to your app's features |
| `index.md` | Landing page linking to all three pages |
| `_config.yml` | Jekyll config for GitHub Pages |

## What it detects

The skill scans your codebase for:

- **Third-party SDKs**: Supabase, Firebase, OpenAI, RevenueCat, Stripe, Google Sign-In, Apple Sign-In, analytics SDKs
- **Device permissions**: Camera, Photo Library, Location, Microphone, Contacts, Calendar, Face ID
- **AI features**: OpenAI imports, CoreML models, Vision framework
- **Subscriptions**: RevenueCat, StoreKit2 product identifiers
- **Data models**: User profile fields, content types, public/shared content

## Requirements

- [Claude Code](https://claude.com/claude-code) CLI
- [GitHub CLI](https://cli.github.com/) (`gh`) — authenticated
- A GitHub account
- An iOS project to scan

## Example output

After running, you'll have a GitHub Pages site at:

```
https://<username>.github.io/<repo-name>/privacy
https://<username>.github.io/<repo-name>/terms
https://<username>.github.io/<repo-name>/support
```

These URLs are ready to paste into App Store Connect.
