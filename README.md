# 📅 LifeSynced

**Unified calendar that syncs work Outlook and personal iCloud calendars into one view.** Overlap detection, smart deduplication, timezone switching—accessible from any device.

---

## 🚀 See It Running

### Option A: Auto-Generate Server Scripts (Recommended)

In Cursor Chat, type:

```
@Generate-server-scripts.md @LifeSynced
```

This creates `start-servers.sh`, `stop-servers.sh`, and `check-servers.sh` for one-command startup.

### Option B: Manual Quick Start

```bash
cd calendar-ui
npm install
cp env.example .env.local
npm run dev
```

Open **http://localhost:3002** in your browser.

**Note:** Full functionality requires Supabase + ICS calendar URLs—see Environment Variables below.

---

## ❓ Why LifeSynced?

Microsoft Outlook doesn't support adding Apple iCloud calendars—only Outlook.com, Hotmail, Live, MSN, or Google. For iPhone users who rely on Apple Calendar for personal events, work and personal calendars can't be viewed together.

**LifeSynced** pulls from both Outlook and iCloud ICS feeds into a unified view.

## ✨ Features

- **Unified view** — Combine work Outlook and personal iCloud calendars
- **Cloud-first** — Supabase (PostgreSQL) backend with Vercel deployment
- **Time-grid views** — 24-hour grid with Day, Week, and 4-Week modes
- **Overlap detection** — Highlight conflicts between work and personal
- **Smart deduplication** — Avoid duplicate events across sync sources
- **Ignore events** — Hide recurring series or individual occurrences
- **Mobile-friendly** — Day view on mobile, Week view on desktop
- **Timezone selector** — Switch timezones when traveling

## 🔑 Environment Variables

Create `calendar-ui/.env.local`:

| Variable | Required | Description |
|----------|----------|-------------|
| `NEXT_PUBLIC_SUPABASE_URL` | ✅ | Supabase project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | ✅ | Supabase service role key |
| `OUTLOOK_ICS_URL` | ✅ | Outlook calendar ICS feed URL |
| `APPLE_CALENDAR_ICS_URL_1` | ✅ | iCloud calendar ICS URL |
| `APPLE_CALENDAR_ICS_URL_2` | | Second iCloud calendar (optional) |
| `APP_PASSWORD` | ✅ | App authentication password |
| `PERSONAL_REVEAL_PASSWORD` | | Password to reveal personal event names |

### Database Setup

Run `calendar-ui/supabase/schema.sql` in your Supabase SQL Editor.

## 🚢 Deployment

Deploy to Vercel:

```bash
cd calendar-ui
vercel --prod
```

Vercel cron syncs calendars daily at 6 AM UTC (configured in `vercel.json`).

---

## 💭 What I Learned

The hardest part: deciphering future events from recurring series that accumulated one-off adjustments over time. But the design decision that unlocked real utility: privacy-first by default. Showing "[Personal Event]" with password-gated reveal made the tool safe to demo with any audience—expanding both reach and utility.

## 🔮 What's Next

Adding **an AI agent that proactively reminds me of important life events** (anniversaries, birthdays) and helps me plan ahead—suggesting celebrations, curating gift ideas, even buying small gifts on my behalf. Agentic commerce in practice.

---

**Status:** Active  
**Stack:** Next.js 14 · TypeScript · Supabase · Tailwind · Vercel Cron

See `CLAUDE.md` for detailed technical setup and development commands.
