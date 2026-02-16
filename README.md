# LocaleLens Next.js Demo

A minimal i18n setup for Next.js using [LocaleLens](https://localelens.ai) — translation infrastructure for developers. Update translations in production without redeploying.

No translation framework.
No JSON files.
Just `fetch()`.

> **Looking for a full-featured example?** See the [TanStack Start demo](https://github.com/localelens/localelens-demo-tanstack-start) for SSR, authentication, and protected routes.

---

## What This Demo Is?

**This demo shows:**
- How to fetch translations from LocaleLens at runtime
- How to integrate i18n into Next.js server components
- How locale switching can work without routing or frameworks

**This demo intentionally avoids:**
- Translation frameworks (`next-intl`, `next-i18next`, etc.)
- File-based messages
- Locale-based routing (`/en`, `/de`)
- Client-side translation state       

---

## Quick start

```bash
npm install
mv .env.example .env.local
# edit .env.local with your project ID and API key
npm run dev
```

### Need a LocaleLens account?

1. Sign up for free at [localelens.ai](https://localelens.ai)
2. Create a project (maybe call it `Next.js Demo`)
3. Add some translations
4. Create an API key
5. Copy your project ID and API key to `.env.local`

No credentials are included in this repository. LocaleLens is the source of truth.

---

## How it works

Translations are fetched server-side with a simple helper:

```typescript
// src/lib/i18n.ts 
const { t, has } = await getTranslations(locale);

t("app.title");    // returns translation, or the key if missing
has("app.title");  // true/false
```

The locale comes from a `NEXT_LOCALE` cookie (defaults to `"en"`). Switching languages sets the cookie via a server action, which triggers a server re-render. That's the entire loop.

Next.js caches the fetch for 60 seconds, so repeated requests are fast.

---

## Project structure

```
src/
├── app/
│   ├── layout.tsx            # fetches translations, renders header
│   ├── page.tsx              # home page
│   ├── about/page.tsx        # another page (proves it scales)
│   └── actions/set-locale.ts # server action for switching
├── components/
│   └── locale-switcher.tsx   # the EN/DE buttons
└── lib/
    └── i18n.ts               # getTranslations() lives here
```

---

## Optional: Import Demo Translations

This repository includes [`docs/localelens-demo-translations.json`](docs/localelens-demo-translations.json) which can be imported into a new LocaleLens project to instantly populate all demo translations.

📄 [View translations](docs/localelens-demo-translations.json) · 📥 [Download](https://raw.githubusercontent.com/localelens/localelens-demo-nextjs/main/docs/localelens-demo-translations.json) (right-click → Save As)

This file exists **only to make the demo easy to reproduce**. The app itself fetches translations from LocaleLens at runtime — it does **not** read from this JSON file.

🔗 Learn more at https://localelens.ai  
📚 Documentation: https://localelens.ai/docs
