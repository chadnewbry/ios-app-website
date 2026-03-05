---
name: app-legal-pages
description: Create a GitHub Pages site with Privacy Policy, Terms of Service, and Support pages for App Store submission. Use when the user needs legal pages, privacy policy, terms of service, or a support page for their mobile app.
trigger: privacy policy, terms of service, legal pages, app store pages, support page, create privacy policy, create terms, app store submission
user-invocable: true
---

# App Legal Pages Skill

Generate and deploy Privacy Policy, Terms of Service, and Support pages for an iOS app using GitHub Pages.

## Workflow

### Phase 1: Gather Information

Collect the information needed to generate the pages. Infer as much as possible from the codebase, then ask the user to confirm and fill in the gaps.

#### Step 1a: Infer from the iOS codebase

Scan the project to detect:

**App name:**
- Check Info.plist for `CFBundleDisplayName` or `CFBundleName`
- Check the Xcode project directory name
- Check any marketing/branding files

**Third-party services — grep Swift files, Package.swift, and Podfile/Podfile.lock for SDK imports:**
- `import Supabase` or `import Supabase` → Supabase (database/auth/storage)
- `import Firebase` or `import FirebaseAuth` → Firebase
- `import OpenAIKit` or `OpenAI` or `ChatGPT` → OpenAI (AI features)
- `import RevenueCat` or `import RevenueCatUI` → RevenueCat (subscriptions)
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

**Data types collected — scan Swift models, Supabase/Firebase schema files, and CoreData models for:**
- User profile fields (email, name, photo, etc.)
- Content types (photos, text, uploads)
- Social/community features

**AI features:**
- OpenAI imports, CoreML model files (`.mlmodel`), Vision framework usage
- Check if photos are sent to an AI API

**Subscriptions/IAP:**
- RevenueCat, StoreKit2 imports, product identifiers

#### Step 1b: Infer automatically

Run these commands:
```bash
# GitHub username
gh api user --jq '.login'

# Today's date for effective date
date '+%B %d, %Y'
```

Default the repo name to a slugified version of the app name (e.g., "Bouquet Book" → "bouquet-book").

#### Step 1c: Ask the user

Use `AskUserQuestion` to gather what can't be inferred. Combine into a single question block where possible.

**Always ask:**
1. Business/entity name (e.g., "Chad Newbry, LLC" or "John Smith")
2. Contact email address
3. Confirm the detected app name (or ask if not found)

**Ask if unclear from codebase scan:**
4. Whether the app has user-generated public content
5. Whether analytics or ad tracking SDKs are used
6. Whether location data is collected (if no Info.plist key found)
7. Preferred repo name (suggest the slugified app name as default)

**Present the detected info for confirmation:**
Show the user a summary of what was detected (third-party services, permissions, data types, AI features, subscriptions) and ask them to confirm or correct it.

### Phase 2: Generate Pages

Read the templates from `references/templates.md` (relative to this skill file). Fill in all `{{PLACEHOLDER}}` variables with the gathered information.

**Generate these files in a temporary directory (e.g., `/tmp/app-legal-pages-<appname>/`):**

1. **`_config.yml`** — Jekyll configuration
   - Set `title` to the app name
   - Set `description` to a one-line app summary
   - Set `theme` to `minima`
   - Set `baseurl` to `/<repo-name>`

2. **`index.md`** — Landing page with links to the three pages

3. **`privacy.md`** — Privacy Policy
   - Fill in all detected data collection, third-party services, permissions
   - Include AI disclaimer section only if AI features detected
   - Skip analytics section if no analytics SDKs detected

4. **`terms.md`** — Terms of Service
   - Include subscription terms only if IAP/subscriptions detected
   - Include AI disclaimer only if AI features detected
   - Include user-generated content section only if UGC detected

5. **`support.md`** — Support page with contact info and FAQ
   - Generate FAQ items based on detected app features
   - Always include: account deletion, subscription management, data privacy
   - Add AI accuracy FAQ if AI features detected

**Show the user the generated files and ask for approval before deploying.**

### Phase 3: Deploy to GitHub Pages

After user approval:

```bash
# Initialize and push
cd /tmp/app-legal-pages-<appname>
git init
git add .
git commit -m "Add privacy policy, terms of service, and support pages"

# Create the GitHub repo and push
gh repo create <repo-name> --public --source=. --push

# Enable GitHub Pages on master branch
gh api repos/<username>/<repo-name>/pages -X POST -f "source[branch]=master" -f "source[path]=/"
```

Wait a moment, then verify the pages are live:
```bash
# Check GitHub Pages build status
gh api repos/<username>/<repo-name>/pages --jq '.status'
```

Use `WebFetch` to verify at least one page loads correctly at `https://<username>.github.io/<repo-name>/privacy`.

Tell the user the URLs:
- Privacy Policy: `https://<username>.github.io/<repo-name>/privacy`
- Terms of Service: `https://<username>.github.io/<repo-name>/terms`
- Support: `https://<username>.github.io/<repo-name>/support`

### Phase 4: Update In-App URLs (Optional)

Ask the user if they want to update URLs in their iOS source code.

If yes:
1. Use `Grep` to find existing privacy/terms/support URLs in the codebase (look for patterns like `privacy`, `terms`, `support` in URL strings)
2. Replace old URLs with the new GitHub Pages URLs
3. Update any `mailto:` links to use the contact email provided
4. Show the user the changes made

If no, just display the URLs for the user to update manually.
