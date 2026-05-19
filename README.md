# Digital CS Onboarding Dashboard

A structured 3-week onboarding dashboard for SmartBear's Digital Customer Success team. Built as a single static HTML file with Supabase for persistent storage.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `index.html` | The entire frontend — one self-contained file, no build tools needed |
| `supabase_backend.sql` | Run once in Supabase SQL Editor to set up all tables, triggers, and policies |
| `README.md` | This file |

---

## Features

- **Onboarding Plan** — 3-week structured plan with expandable weeks and checkable tasks
- **Sessions** — Session list per week with inline Mark as complete button and timestamps
- **Tools Access** — Per-tool dropdown (Pending / Access Granted) with a progress bar
- **Share tokens** — Owner generates access tokens; shared users log in with their token
- **Supabase sync** — All progress, timestamps, tool access, and tokens stored in a real database and synced across devices

---

## Setup

### 1. Supabase (backend)

1. Go to [supabase.com](https://supabase.com) and create a free account
2. Create a new project
3. Go to **SQL Editor → New query**
4. Paste the entire contents of `supabase_backend.sql` and click **Run**
5. You should see `dcs_progress = 1, dcs_tokens = 0, dcs_audit = 0`
6. Go to **Project Settings → API** and copy:
   - **Project URL** (e.g. `https://xyzxyz.supabase.co`)
   - **anon public** key

### 2. Frontend (index.html)

Open `index.html` and find these two lines near the top of the `<script>` tag:

```js
const SUPABASE_URL  = "YOUR_SUPABASE_URL";
const SUPABASE_ANON = "YOUR_SUPABASE_ANON_KEY";
```

Replace with your actual values from step 1.

### 3. GitHub Pages (hosting)

1. Push this repo to GitHub (must be **Public** for free Pages)
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch**, branch `main`, folder `/ (root)`
4. Click **Save**
5. Your dashboard will be live at `https://YOUR-USERNAME.github.io/REPO-NAME/`

---

## How sharing works

1. Open the dashboard — you are the owner by default
2. Click **🔗 Share** in the top bar
3. Click **Generate new access token** — copy the link
4. Send the link to your new hire
5. They open the link and enter the token to access the dashboard
6. Revoke any token at any time from the Share modal

---

## Database schema

| Table | Purpose |
|-------|---------|
| `dcs_progress` | Single row storing task progress, timestamps, and tool access as JSONB |
| `dcs_tokens` | One row per active share token |
| `dcs_audit` | Automatic log of every task toggle, tool status change, and token event |

### Useful Supabase queries

```sql
-- See all task statuses
select * from dcs_task_status order by week, task_index;

-- See completed tasks only
select * from dcs_task_status where is_done = true order by completed_at;

-- See tool access statuses
select * from dcs_tool_status;

-- Full audit history
select * from dcs_audit_log limit 100;

-- Active share tokens
select token, created_at from dcs_tokens order by created_at;
```
