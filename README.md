# Ta-do

**Tell me what to do next — and let me trust nothing fell through the cracks.**

Ta-do is a capture-first app for everything in your head: thoughts, tasks and goals. Get things down the moment they come to mind, by typing, by voice or with Siri. Sort them later, and always come back to a short list of what's next.

Available on the web at **[ta-do.vercel.app](https://ta-do.vercel.app)** and as an iOS app.

## What it does

- **Brain dump** — capture anything as a Thought, Task or Goal, and change its type later
- **Capture several at once** — paste a list and every line becomes its own task, after you've had a chance to review them
- **Categories** — group entries under Spiritual, Physical, Psychological and Career, or your own labels
- **Today's list** — see what's due today, what's overdue and what's coming up; drag to reorder, swipe to complete or archive
- **Repeating tasks** — daily, weekly, monthly or on a custom schedule
- **Library** — every entry in one place, filterable by type, including archived ones
- **Notes** on any entry
- **Light, dark or automatic theme**, and a timezone that follows your device or stays fixed
- **Sign in** with an email link or Google

### Capture from anywhere

- **Siri** — "Brain dump in Ta-do", "Add a task to Ta-do", "Talk to Ta-do"
- **Voice commands** — set the type, labels, due date, repeat and reminders as you speak ("… tomorrow at 6, label as family"), with your own custom phrases. You always get a review card before anything is saved
- **iOS Shortcut** — import [`Brain Dump.shortcut`](public/Brain%20Dump.shortcut); the app walks you through connecting it on first launch

## Setup

You'll need Node.js 18+, a [Supabase](https://supabase.com) project and the [Supabase CLI](https://supabase.com/docs/guides/cli).

1. **Clone and install**

   ```bash
   git clone https://github.com/epz999/ta-do.git
   cd ta-do
   npm install
   ```

2. **Add your Supabase keys**

   ```bash
   cp .env.example .env.local
   ```

   Fill in `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` from your Supabase project settings.

3. **Set up the database.** First enable the `pg_cron` extension in Supabase (it's needed for repeating tasks). Then run:

   ```bash
   supabase link --project-ref <your-project-ref>
   supabase db push
   supabase functions deploy capture-entry
   ```

4. **Configure sign-in.** In Supabase, go to **Authentication → URL Configuration** and add your local URL and your production URL under **Redirect URLs**. For Google sign-in, also enable the Google provider.

5. **Run it**

   ```bash
   npm run dev
   ```

### iOS app

You'll need a Mac with Xcode, and [XcodeGen](https://github.com/yonaskolb/XcodeGen) (`brew install xcodegen`).

```bash
cd ios
./scripts/write-secrets.sh   # reuses the keys from .env.local
xcodegen
open TaDo.xcodeproj
```

Pick a simulator or your iPhone and press Run. Add `tado://auth-callback` to the Supabase Redirect URLs so sign-in can return to the app. More detail is in [`ios/README.md`](ios/README.md).
