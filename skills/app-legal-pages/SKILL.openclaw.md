---
name: app-legal-pages
description: Create a GitHub Pages site with Privacy Policy, Terms of Service, and Support pages for App Store submission. Use when the user needs legal pages, privacy policy, terms of service, or a support page for their mobile app.
---

# App Legal Pages Skill (OpenClaw)

Generate and deploy Privacy Policy, Terms of Service, and Support pages for an iOS app using GitHub Pages.

## Workflow

### Phase 1: Gather Information

Collect the information needed to generate the pages. Infer as much as possible from the codebase.

#### Step 1a: Infer from the iOS codebase

Use `exec` and `read` to scan the project and detect:

**App name:**
- Check Info.plist for `CFBundleDisplayName` or `CFBundleName`
- Check the Xcode project directory name

**Third-party services — grep Swift files, Package.swift, and Podfile for SDK imports:**
- `import Supabase` → Supabase (database/auth/storage)
- `import Firebase` or `import FirebaseAuth` → Firebase
- `import OpenAIKit` or `OpenAI` → OpenAI (AI features)
- `import RevenueCat` → RevenueCat (subscriptions)
- `import Stripe` → Stripe (payments)
- `import Amplitude` or `import Mixpanel` or `import FirebaseAnalytics` → Analytics
- `import GoogleSignIn` → Google Sign-In
- `ASAuthorizationAppleIDProvider` → Apple Sign-In

**Device permissions — parse Info.plist for usage description keys:**
- `NSCameraUsageDescription` → Camera access
- `NSPhotoLibraryUsageDescription` → Photo Library access
- `NSLocationWhenInUseUsageDescription` or `NSLocationAlwaysUsageDescription` → Location
- `NSMicrophoneUsageDescription` → Microphone
- `NSContactsUsageDescription` → Contacts
- `NSCalendarsUsageDescription` → Calendar
- `NSFaceIDUsageDescription` → Face ID

**Data types — scan Swift models, schema files, and CoreData/SwiftData models for:**
- User profile fields (email, name, photo)
- Content types (photos, text, uploads)
- Social/community features

**AI features:**
- OpenAI imports, CoreML model files (`.mlmodel`), Vision framework usage

**Subscriptions/IAP:**
- RevenueCat, StoreKit2 imports, product identifiers

#### Step 1b: Infer automatically

```bash
# GitHub username
gh api user --jq '.login'

# Today's date for effective date
date '+%B %d, %Y'
```

Default the repo name to a slugified version of the app name (e.g., "Bouquet Book" → "bouquet-book").

#### Step 1c: Fill in defaults or ask

**Required info (use these defaults if running autonomously in the pipeline):**
- Business name: **Chad Newbry, LLC**
- Contact email: **chad.newbry@gmail.com**
- GitHub username: **chadnewbry**

If running interactively (main session), confirm these with the user and ask about anything unclear from the codebase scan.

### Phase 2: Generate Pages

Read the templates from `references/templates.md` (relative to this skill file). Fill in all `{{PLACEHOLDER}}` variables.

**Generate files in `/tmp/app-legal-pages-<appname>/`:**

1. **`_config.yml`** — Jekyll config (title, description, theme: minima, baseurl)
2. **`index.md`** — Landing page with links
3. **`privacy.md`** — Privacy Policy (include detected data collection, services, permissions; AI section only if AI features detected)
4. **`terms.md`** — Terms of Service (subscription terms only if IAP detected; AI disclaimer only if AI features; UGC section only if applicable)
5. **`support.md`** — Support page with contact info and FAQ (generate FAQ from detected features; always include: account deletion, subscription management, data privacy)

If running interactively, show files and ask for approval before deploying.

### Phase 3: Deploy to GitHub Pages

```bash
cd /tmp/app-legal-pages-<appname>
git init
git add .
git commit -m "Add privacy policy, terms of service, and support pages"
gh repo create <repo-name> --public --source=. --push
gh api repos/<username>/<repo-name>/pages -X POST -f "source[branch]=master" -f "source[path]=/"
```

Verify deployment:
```bash
gh api repos/<username>/<repo-name>/pages --jq '.status'
```

Use `web_fetch` to verify pages load at `https://<username>.github.io/<repo-name>/privacy`.

Report the URLs:
- Privacy Policy: `https://<username>.github.io/<repo-name>/privacy`
- Terms of Service: `https://<username>.github.io/<repo-name>/terms`
- Support: `https://<username>.github.io/<repo-name>/support`

### Phase 4: Update In-App URLs (Optional)

If running as part of the pipeline (engineer agent), update URLs in the iOS source code:
1. Grep for existing privacy/terms/support URLs
2. Replace with new GitHub Pages URLs
3. Update any `mailto:` links to use the contact email

If running interactively, ask before making changes.
