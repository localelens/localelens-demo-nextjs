# LocaleLens Next.js Demo

A minimal i18n setup for Next.js using [LocaleLens](https://localelens.ai).

No translation framework.
No JSON files.
Just `fetch()`.

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

### Quick Import (Optional)

To quickly populate your LocaleLens project, copy and import this JSON via the LocaleLens UI.

This JSON is **for import only**. The app itself does **not** read from JSON files.

This matches the keys used in the demo UI.

```json
{
  "en": {
    "app.title": "LocaleLens Demo",
    "home.title": "Welcome",
    "home.description": "This demo shows how LocaleLens replaces file-based i18n with a simple API fetch.",
    "home.how_it_works": "How it works",
    "home.step_1": "Translations are fetched from LocaleLens at request time",
    "home.step_2": "Next.js caches the response for 60 seconds",
    "home.step_3": "No JSON files, no framework, just fetch()",
    "home.missing_key_title": "Missing key fallback",
    "home.missing_key_description": "When a key doesn't exist, the key itself is returned:",
    "nav.about": "About",
    "nav.home": "Home",
    "about.title": "About This Demo",
    "about.description": "This is a minimal i18n implementation for Next.js using LocaleLens.",
    "about.no_framework": "No Framework",
    "about.no_framework_detail": "No next-intl, no next-i18next. Just a simple getTranslations() function.",
    "about.server_first": "Server First",
    "about.server_first_detail": "Translations are fetched in server components. No client-side state.",
    "about.cache_friendly": "Cache Friendly",
    "about.cache_friendly_detail": "Next.js fetch revalidation handles caching automatically."
  },
  "de": {
    "app.title": "LocaleLens Demo",
    "home.title": "Willkommen",
    "home.description": "Diese Demo zeigt, wie LocaleLens dateibasiertes i18n durch einen einfachen API-Aufruf ersetzt.",
    "home.how_it_works": "So funktioniert es",
    "home.step_1": "Übersetzungen werden zur Laufzeit von LocaleLens abgerufen",
    "home.step_2": "Next.js cached die Antwort für 60 Sekunden",
    "home.step_3": "Keine JSON-Dateien, kein Framework – einfach fetch()",
    "home.missing_key_title": "Fehlender Key",
    "home.missing_key_description": "Wenn ein Key nicht existiert, wird der Key selbst zurückgegeben:",
    "nav.about": "Über",
    "nav.home": "Start",
    "about.title": "Über diese Demo",
    "about.description": "Eine minimale i18n-Implementierung für Next.js mit LocaleLens.",
    "about.no_framework": "Kein Framework",
    "about.no_framework_detail": "Kein next-intl, kein next-i18next. Nur eine einfache getTranslations()-Funktion.",
    "about.server_first": "Server-First",
    "about.server_first_detail": "Übersetzungen werden in Server-Komponenten geladen. Kein Client-State.",
    "about.cache_friendly": "Cache-freundlich",
    "about.cache_friendly_detail": "Die Fetch-Revalidierung von Next.js übernimmt das Caching automatisch."
  }
}
```

🔗 Learn more at https://localelens.ai  
📚 Documentation: https://localelens.ai/docs
