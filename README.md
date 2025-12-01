# LifeSynced

![Type](https://img.shields.io/badge/Type-App-blue)
![Status](https://img.shields.io/badge/Status-Active-green)
![Stack](https://img.shields.io/badge/Stack-Next.js%20%7C%20Supabase-blue)
![Deploy](https://img.shields.io/badge/Deploy-Vercel-black)

Unified calendar application that syncs work Outlook and personal iCloud calendars into a cloud database (Supabase) and surfaces everything in a modern web UI with overlap detection, smart deduplication, and family sharing.

## Why LifeSynced?

Microsoft Outlook (commonly used in enterprise/work environments) does not support adding Apple iCloud calendars. The "Accounts" settings only allow connecting Outlook.com, Hotmail, Live, MSN, or Google—**no Apple/iCloud option exists**. For iPhone and iCloud users who rely on Apple Calendar for personal events, this creates a fragmented experience where work and personal calendars cannot be viewed together.

LifeSynced solves this by pulling from both Outlook and iCloud ICS feeds into a unified view.

## Features

- **Unified view:** Combine work Outlook and personal iCloud calendars into one timeline.
- **Cloud-first:** Supabase (PostgreSQL) backend with Vercel deployment—accessible from any device.
- **Time-grid views:** 24-hour grid (0000–2400) with Day, Week, and 4-Week modes.
- **Overlap detection:** Highlight conflicts between work and personal events.
- **Smart deduplication:** Avoid duplicate events across sync sources.
- **Ignore events:** Hide recurring series or individual occurrences.
- **Mobile-friendly:** Responsive design—Day view on mobile, Week view on desktop.
- **Timezone selector:** Switch timezones when traveling.
- **Event tooltips:** Hover/tap for event details.
- **Real-time updates:** No caching issues—changes reflect immediately.

## Quick Start (Cloud Deployment)

### 1. Set up Supabase

1. Create a project at [supabase.com](https://supabase.com)
2. Run the schema from `calendar-ui/supabase/schema.sql` in SQL Editor
3. Copy your project URL and service role key

### 2. Configure environment

Create `calendar-ui/.env.local`:

```bash
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
OUTLOOK_ICS_URL=https://outlook.office365.com/owa/calendar/...
APPLE_CALENDAR_ICS_URL_1=https://p123-caldav.icloud.com/...
APPLE_CALENDAR_ICS_URL_2=https://p123-caldav.icloud.com/...
```

### 3. Deploy to Vercel

```bash
cd calendar-ui
vercel --prod
```

Add the same environment variables in Vercel project settings.

### 4. Set up automatic sync

Vercel cron syncs daily at 6 AM UTC (configured in `vercel.json`).  
Manual sync: click the 🔄 button in the UI.

---

## Local Development

```bash
cd calendar-ui
npm install
npm run dev
```

Open `http://localhost:3002`

---

## Architecture

```
┌─────────────────┐     ┌──────────────┐     ┌─────────────┐
│  Outlook ICS    │────▶│              │────▶│             │
│  iCloud ICS     │     │  Next.js API │     │  Supabase   │
│  (Calendar      │     │  Routes      │     │  PostgreSQL │
│   Feeds)        │     │  (TypeScript)│     │             │
└─────────────────┘     └──────────────┘     └─────────────┘
                               │
                               ▼
                        ┌──────────────┐
                        │   React UI   │
                        │  (Next.js)   │
                        └──────────────┘
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service role key (server-side only) |
| `OUTLOOK_ICS_URL` | Outlook calendar ICS feed URL |
| `APPLE_CALENDAR_ICS_URL_1` | First iCloud calendar ICS URL |
| `APPLE_CALENDAR_ICS_URL_2` | Second iCloud calendar ICS URL (optional) |

## Database Schema

- **`appointments`** – All calendar events with subject, times, location, organizer, source
- **`ignored_base_ids`** – Recurring series to hide
- **`ignored_event_ids`** – Individual occurrences to hide
- **`sync_metadata`** – Last sync timestamps

Row Level Security (RLS) is enabled for data protection.

## API Routes

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/events` | GET | Fetch events (filtered by ignored lists) |
| `/api/sync` | POST | Trigger calendar sync |
| `/api/ignored-base-ids` | GET/POST/DELETE | Manage ignored series |
| `/api/ignored-event-ids` | GET/POST/DELETE | Manage ignored occurrences |

## Troubleshooting

- **Events not updating:** Cleared via proper cache headers—should update immediately
- **Missing recurring events:** Sync expands up to 500 occurrences per series
- **Timezone wrong:** Select correct timezone from the 🌐 dropdown

## Development

See `CLAUDE.md` for detailed project structure and commands.

---

**Status:** Active  
**Purpose:** Personal/family productivity—unified calendar view with cloud sync
