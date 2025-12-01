# LifeSynced – Project Plan

## Executive Summary

LifeSynced is a personal/family productivity tool that unifies work (Outlook) and personal (iCloud) calendars into a single cloud-hosted view, detects conflicts between them, and gives users control to declutter what they see. The system pulls events from ICS feeds, parses them in TypeScript, stores them in Supabase (PostgreSQL), and exposes everything through a modern Next.js web UI deployed on Vercel—accessible from any device.

---

## Problem & Goals

### Problem

- **Fragmented calendar management:** Work (Outlook) and personal (iCloud) calendars live in separate systems with no native unified view.
- **Hidden conflicts:** It is hard to spot scheduling conflicts between work commitments and personal obligations across those silos.
- **Device-bound solutions:** Local-only tools don't support family sharing or multi-device access.

### Primary Objectives

- **Unified calendar view:** Single, cohesive interface displaying both work and personal events.
- **Conflict detection:** Automatically detect and highlight overlaps between work and personal events.
- **Cloud-first:** Accessible from any device via web browser, shareable with family.
- **User-centric control:** Selectively ignore or hide events to keep the view focused.

### Success Criteria

- All relevant calendar events visible in one place.
- Overlaps between work and personal calendars clearly identified.
- No duplicate events from multiple sync sources.
- Accessible from any device (phone, tablet, desktop).
- Shareable with family members.
- Sync runs reliably and automatically.
- Web interface is intuitive, responsive, and pleasant to use.

---

## Architecture

### Cloud Stack

```
┌─────────────────┐     ┌──────────────┐     ┌─────────────┐
│  Calendar Feeds │     │   Vercel     │     │  Supabase   │
│  - Outlook ICS  │────▶│  Next.js API │────▶│  PostgreSQL │
│  - iCloud ICS   │     │  (TypeScript)│     │  (Cloud DB) │
└─────────────────┘     └──────────────┘     └─────────────┘
                               │
                               ▼
                        ┌──────────────┐
                        │   React UI   │
                        │  (Next.js)   │
                        │  Any Device  │
                        └──────────────┘
```

### Components

1. **Data Sources:** Outlook ICS feed, iCloud public calendar ICS URLs
2. **Sync Layer:** TypeScript API route (`/api/sync`) using `ical.js` for parsing
3. **Storage:** Supabase PostgreSQL with RLS enabled
4. **Presentation:** Next.js React app with TailwindCSS
5. **Hosting:** Vercel (frontend + API routes + cron jobs)

---

## Functional Requirements

### Calendar Synchronization

- Fetch events from Outlook ICS feed
- Fetch events from multiple iCloud ICS feeds
- Parse ICS with `ical.js` (TypeScript)
- Expand recurring events (RRULE) up to 500 occurrences
- Store events in Supabase with deduplication (upsert on ID)
- Automatic daily sync via Vercel cron (6 AM UTC)
- Manual sync via UI button

### Data Management

- Centralized PostgreSQL database (Supabase)
- UTC timestamp storage for consistency
- Row Level Security for data protection
- Deduplication by event ID
- Track event source (ics, apple_calendar)

### Conflict Detection

- Compare time ranges between work and personal events
- Visual indicators (orange highlight) for overlapping events
- Toggle to show only conflicts
- Exclude all-day/multi-day "Free" events from overlap calculation

### User Interface

- **Views:** Day, Week (default on desktop), 4-Week
- **Time grid:** Full 24-hour display (0000–2400)
- **Responsive:** Day view default on mobile (<768px)
- **Features:**
  - Color-coded events (blue=work, green=personal, orange=overlap)
  - Sticky day headers
  - Side-by-side display for overlapping events
  - Hide weekends toggle
  - Timezone selector with persistence
  - Event tooltips on hover/tap
  - Ignore events (series or individual occurrence)
  - Manual sync button
  - Real-time updates (no caching issues)

### Event Management

- Ignore recurring series (all future occurrences)
- Ignore individual occurrences
- View/manage ignored events list
- Unignore events

---

## Non-Functional Requirements

- **Accessibility:** Cloud-hosted, accessible from any device
- **Shareability:** Family members can access same URL
- **Reliability:** Graceful handling of sync failures
- **Performance:** Fast queries via database indexes
- **Privacy:** RLS enabled; service role key server-side only
- **Real-time:** No caching—changes reflect immediately

---

## Data Sources

### Outlook (Work)
- **ICS Feed:** Published calendar URL from Outlook on the Web
- Shows: Free/Busy/Tentative status (titles may be hidden for privacy)

### iCloud (Personal)
- **Public Calendar ICS:** Share calendar as public in iCloud settings
- Shows: Full event details (titles, times, locations)

---

## Deployment

### Vercel
- Automatic deployments from GitHub
- Environment variables in project settings
- Cron job for daily sync (`vercel.json`)

### Supabase
- PostgreSQL database
- Schema in `calendar-ui/supabase/schema.sql`
- RLS enabled for security

---

## Recently Implemented

- ✅ Cloud migration from SQLite to Supabase
- ✅ TypeScript sync logic (no Python dependency for cloud)
- ✅ Vercel deployment with cron jobs
- ✅ Row Level Security (RLS) enabled
- ✅ Proper cache-busting (no stale data issues)
- ✅ Recurring event expansion (500 occurrences max)
- ✅ Timezone-aware event positioning
- ✅ Mobile-responsive design
- ✅ Event tooltips on hover/tap
- ✅ Ignore series or individual occurrences
- ✅ Side-by-side overlapping event display

---

## Future Considerations

### Potential Enhancements

- User authentication (Supabase Auth) for multi-user support
- Google Calendar integration
- Push notifications for conflicts
- PWA for mobile home screen
- Calendar export functionality
- Event search/filter
- Dark mode

### Scalability

- Current design optimized for single user/family
- Supabase free tier sufficient for personal use
- Can upgrade Supabase plan for higher usage
