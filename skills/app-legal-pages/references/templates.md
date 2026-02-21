# Page Templates

Templates for generating GitHub Pages legal pages. Replace all `{{PLACEHOLDER}}` variables with actual values.

---

## `_config.yml`

```yaml
title: {{APP_NAME}}
description: {{APP_DESCRIPTION}}
theme: minima
baseurl: /{{REPO_NAME}}
```

---

## `index.md`

```markdown
---
layout: default
title: {{APP_NAME}}
---

# {{APP_NAME}}

{{APP_DESCRIPTION}}

- [Privacy Policy](privacy)
- [Terms of Service](terms)
- [Support](support)
```

---

## `privacy.md`

```markdown
---
layout: default
title: Privacy Policy
---

# Privacy Policy

**Effective Date:** {{EFFECTIVE_DATE}}

**{{BUSINESS_NAME}}** ("we", "us", or "our") operates the {{APP_NAME}} mobile application (the "App"). This Privacy Policy explains how we collect, use, and protect your information.

## Information We Collect

### Account Information
{{ACCOUNT_DATA_SECTION}}

### {{PRIMARY_CONTENT_TYPE}} Data
{{CONTENT_DATA_SECTION}}

{{SUBSCRIPTION_SECTION_PRIVACY}}

## Information We Do NOT Collect
{{NOT_COLLECTED_SECTION}}

## How We Use Your Information
- To provide the App's core features{{CORE_FEATURES_LIST}}
- To authenticate your account
{{AI_USAGE_LINE}}

## Third-Party Services

We use the following third-party services:

| Service | Purpose |
|---------|---------|
{{THIRD_PARTY_TABLE}}

These services have their own privacy policies and may process your data according to their terms.

## Device Permissions

The App may request the following permissions:

{{PERMISSIONS_LIST}}

You can revoke these permissions at any time in your device Settings.

## Data Storage & Security

{{DATA_STORAGE_SECTION}}

## Your Rights

- **Account deletion** — you may delete your account and all associated data by contacting us
- **Data export** — you may request a copy of your data by contacting us
- **Data correction** — you may update your profile information at any time within the App

## Children's Privacy

The App is not directed to children under 13. We do not knowingly collect personal information from children under 13.

## Changes to This Policy

We may update this Privacy Policy from time to time. Changes will be posted on this page with an updated effective date.

## Contact Us

If you have questions about this Privacy Policy, please contact us at:

**Email:** [{{CONTACT_EMAIL}}](mailto:{{CONTACT_EMAIL}})
```

### Privacy Template — Section Fragments

Use these fragments to fill in the section placeholders above.

#### `{{ACCOUNT_DATA_SECTION}}` — Always include applicable items:
```markdown
- **Email address** — used for account creation and communication
- **Name** — used for your profile display
- **Profile photo** — optionally uploaded for your account avatar
```
Omit items not collected by the app. Add items if the app collects additional account data.

#### `{{CONTENT_DATA_SECTION}}` — Customize to the app's content type:
```markdown
- **[Content] photos** — images you upload
- **[AI result type]** — AI-generated results (names, descriptions, etc.)
- **Notes and descriptions** — any text you add to your [content]
```

#### `{{SUBSCRIPTION_SECTION_PRIVACY}}` — Include only if app has subscriptions:
```markdown
### Subscription Information
- Subscription status and entitlements are managed through [RevenueCat and Apple / Apple / provider]. We do not directly collect or store payment information.
```
Omit this section entirely if the app has no subscriptions/IAP.

#### `{{NOT_COLLECTED_SECTION}}` — List what the app does NOT collect:
```markdown
- **Location data** — we do not request or store your location
- **Analytics or tracking** — we do not use third-party analytics or ad tracking SDKs
```
Only list items as "not collected" if they are truly not collected. If the app uses analytics, do not include the analytics line — instead document what analytics are used in the "How We Use" section. If the app collects location, remove that line and add location to the collected data.

#### `{{AI_USAGE_LINE}}` — Include only if app has AI features:
```markdown
- To process AI-powered [feature description, e.g., "flower identification"]
```

#### `{{THIRD_PARTY_TABLE}}` — One row per service:
```markdown
| **Supabase** | Database, file storage, and authentication |
| **OpenAI** | [Content] photos are sent to OpenAI's API for AI-powered [feature] |
| **RevenueCat** | Subscription and in-app purchase management |
| **Apple Sign-In** | Authentication |
| **Google Sign-In** | Authentication |
| **Firebase** | Analytics and crash reporting |
```
Only include services actually detected in the codebase.

#### `{{PERMISSIONS_LIST}}` — One item per permission:
```markdown
- **Camera** — to take photos of [content]
- **Photo Library** — to select existing photos from your device
- **Location** — to [purpose]
- **Microphone** — to [purpose]
```
Only include permissions found in Info.plist.

#### `{{DATA_STORAGE_SECTION}}` — Customize to backend:
```markdown
Your data is stored securely using [backend provider] cloud infrastructure. All data is transmitted over HTTPS, and database access is protected by [security measures, e.g., "row-level security (RLS) policies that ensure you can only access your own data"].
```

---

## `terms.md`

```markdown
---
layout: default
title: Terms of Service
---

# Terms of Service

**Effective Date:** {{EFFECTIVE_DATE}}

These Terms of Service ("Terms") govern your use of the {{APP_NAME}} mobile application (the "App") operated by **{{BUSINESS_NAME}}** ("we", "us", or "our"). By using the App, you agree to these Terms.

## Use of the App

{{APP_DESCRIPTION_TERMS}}

## Accounts

You must create an account to use the App. You are responsible for maintaining the security of your account credentials and for all activity under your account.

{{SUBSCRIPTION_TERMS}}

## User Content

You retain ownership of the photos, notes, and other content you upload to the App ("User Content"). By uploading User Content, you grant us a limited license to store, display, and process it as necessary to provide the App's features.

{{UGC_PUBLIC_CONTENT}}

{{AI_TERMS}}

## Prohibited Uses

You agree not to:
- Use the App for any unlawful purpose
- Upload content that infringes on others' intellectual property
- Attempt to interfere with or disrupt the App's infrastructure
- Reverse-engineer or decompile the App

## Intellectual Property

The App, including its design, code, and branding, is owned by {{BUSINESS_NAME}}.{{IP_ADDITIONAL}}

## Disclaimer of Warranties

The App is provided "as is" without warranties of any kind. We do not guarantee that the App will be error-free{{AI_WARRANTY_ADDITION}}.

## Limitation of Liability

To the maximum extent permitted by law, {{BUSINESS_NAME}} shall not be liable for any indirect, incidental, special, or consequential damages arising from your use of the App.

## Termination

We may suspend or terminate your access to the App at any time for violation of these Terms. You may stop using the App at any time.

## Changes to These Terms

We may update these Terms from time to time. Changes will be posted on this page with an updated effective date. Continued use of the App after changes constitutes acceptance.

## Contact Us

If you have questions about these Terms, please contact us at:

**Email:** [{{CONTACT_EMAIL}}](mailto:{{CONTACT_EMAIL}})
```

### Terms Template — Section Fragments

#### `{{APP_DESCRIPTION_TERMS}}` — One paragraph describing the app:
```markdown
{{APP_NAME}} lets you [primary feature]. You may use the App for personal, non-commercial purposes in accordance with these Terms.
```

#### `{{SUBSCRIPTION_TERMS}}` — Include only if app has subscriptions:
```markdown
## Subscriptions

{{APP_NAME}} offers a free tier and premium subscriptions. Subscriptions are managed through Apple's App Store and processed by [RevenueCat / Apple / provider].

- **Billing** — subscriptions are billed through your Apple ID account
- **Renewal** — subscriptions auto-renew unless cancelled at least 24 hours before the end of the current period
- **Cancellation** — you can cancel anytime in your Apple ID account settings
- **Refunds** — refund requests are handled by Apple per their refund policies
```
Omit this section entirely if the app has no subscriptions.

#### `{{UGC_PUBLIC_CONTENT}}` — Include only if app has public/shared content:
```markdown
You may choose to make [content] publicly visible. You are responsible for any content you make public.
```
Omit if app has no public/community features.

#### `{{AI_TERMS}}` — Include only if app has AI features:
```markdown
## AI-Powered Features

The App uses AI (powered by [provider]) to [feature description]. AI-generated results are provided for informational purposes only and **accuracy is not guaranteed**. Do not rely on [AI result type] for medical, safety, or allergy-related decisions.
```
Omit this section entirely if no AI features.

#### `{{AI_WARRANTY_ADDITION}}` — Append to warranty disclaimer if AI features present:
```markdown
, uninterrupted, or that AI identifications will be accurate
```
If no AI, use:
```markdown
 or uninterrupted
```

#### `{{IP_ADDITIONAL}}` — Any additional IP attributions:
```markdown
 [Dataset/library name] is used under its respective license.
```
Omit if none.

---

## `support.md`

```markdown
---
layout: default
title: Support
---

# Support

Need help with {{APP_NAME}}? We're here for you.

## Contact Us

**Email:** [{{CONTACT_EMAIL}}](mailto:{{CONTACT_EMAIL}})

We typically respond within 24–48 hours.

## Frequently Asked Questions

{{FAQ_ITEMS}}
```

### Support Template — FAQ Fragments

Generate FAQ items based on detected app features. Always include the core items, then add conditional items.

#### Core FAQ items (always include):

```markdown
### How do I delete my account?
Contact us at [{{CONTACT_EMAIL}}](mailto:{{CONTACT_EMAIL}}) and we'll delete your account and all associated data.

### Is my data private?
Yes. {{PRIVACY_SUMMARY}} See our [Privacy Policy](privacy) for full details.
```

#### Conditional FAQ items:

**If AI features detected:**
```markdown
### How do I [use AI feature]?
[Step-by-step description of the AI feature workflow]

### How accurate is the [AI feature]?
{{APP_NAME}} uses AI-powered [feature] which works well for [common cases]. Results are provided for informational purposes and may not always be 100% accurate.
```

**If subscriptions detected:**
```markdown
### How do I upgrade to Premium?
Go to Settings and tap on your subscription status to view Premium plans.

### How do I cancel my subscription?
Subscriptions are managed through Apple. Go to your iPhone Settings > Apple ID > Subscriptions to manage or cancel.
```

**If camera/photo features detected:**
```markdown
### How do I [primary photo action]?
Tap the "+" button, take a photo or choose one from your library, and {{APP_NAME}} will [what happens next].
```
