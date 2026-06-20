# Slack Pulse

A single-file analytics dashboard for Slack workspace data. Drop in your Slack analytics CSV export and instantly get charts, health scores, and behavioural insights — no server, no install, no data ever leaves your browser.

![Overview tab showing KPI cards and message activity charts](https://placeholder.example.com/screenshot.png)

---

## What it does

Slack's built-in analytics pages are useful but limited — you see one metric at a time with no cross-analysis. Slack Pulse loads your exported CSV and renders a full dashboard across four tabs:

| Tab | What's in it |
|---|---|
| **Overview** | KPI cards, message activity, message mix by channel type, membership growth, public channel count, openness trend |
| **Behavior** | Active vs posting people, lurker ratio, poster rate, message density, voice concentration, views vs messages |
| **Health** | Five health scores (Stickiness, Openness, Participation, Activation, Growth), MAP vs membership gap, ghost member count, WAU/MAP stickiness trend, activation rate |
| **Rhythms** | Activity heatmap (GitHub-style), avg messages and active people by day of week, avg activity by day of month |

---

## Getting started

### 1. Export your Slack analytics data

In Slack, go to **your workspace menu → Tools & settings → Analytics → Export**. Choose a date range (up to 12 months recommended) and download the CSV.

> You need Slack Pro, Business+, or Enterprise to access analytics exports.

### 2. Open the dashboard

Download `slack_pulse.html` and open it in any modern browser. No internet connection required after that.

### 3. Load your CSV

Drag and drop the CSV file onto the upload area, or click to browse. The dashboard renders immediately.

---

## Metrics explained

### KPI cards

| Metric | Definition |
|---|---|
| **Avg daily active people** | Mean of the `Daily active people` column across all days in the export |
| **Avg lurker ratio** | Average share of daily active people who read but do not post |
| **WAU/MAP stickiness** | Weekly active users divided by monthly active people — how habitual usage is (0 = drifting, 1 = sticky) |
| **Msgs / active / day** | Average messages sent per active person per day — signal density |
| **% public messages** | Share of messages sent in public channels vs private/DMs |

### Health scores (0–100)

| Score | How it's calculated |
|---|---|
| **Stickiness** | Average WAU÷MAP ratio × 100 |
| **Openness** | Average % of messages sent in public channels |
| **Participation** | Average % of active people who post (inverse of lurker ratio) |
| **Activation** | Rolling 28-day active people as a % of total members |
| **Growth** | Average month-over-month membership growth rate, where 5% per month = score 100 |

### Behavioral charts

- **Active vs posting people** — daily active people vs daily people posting; the gap is your lurker pool
- **Lurker ratio** — the share of active people who only read, not write; lower is more participatory
- **Poster rate %** — % of active people who post at least one message per day
- **Message density** — messages per active person per day; tracks whether conversations are getting richer or thinner
- **Voice concentration** — messages per poster; high values mean a small group is doing most of the talking
- **Views vs messages (public / DMs)** — reading vs writing behaviour by channel type

### Aggregation toggle

Most charts can be switched between **daily**, **weekly**, and **monthly** views using the buttons above each chart. Weekly and monthly views show averages normalised per day so they're comparable across periods.

---

## Data & privacy

- **Nothing is uploaded.** The CSV is read entirely in your browser using the `FileReader` API. It is never sent anywhere.
- **No account required.** Open the file and use it.
- Rows with fewer than 5 total enabled members are excluded from all calculations to avoid misleading metrics during early workspace ramp-up. The date badge shows how many days were excluded and why.

---

## Supported CSV format

Slack Pulse expects the standard CSV produced by Slack's Analytics export. Required columns include:

- `Date`
- `Daily active people`, `Weekly active people`, `Monthly active people`
- `Daily people posting messages`
- `Messages in public channels`, `Messages in private channels`, `Messages in DMs`, `Messages in shared channels`
- `Public channels`, `Total Enabled Membership`
- `Percent of views, public channels`, `Percent of messages, public channels`

If your export is missing columns, some charts will show no data but the rest of the dashboard will still render.

---

## Browser support

Any modern browser works: Chrome, Firefox, Safari, Edge. The file uses ES2020 features (`async/await`, optional chaining, `Intl`) and Chart.js loaded from a CDN, so an internet connection is needed on first load if you want charts to render — after that the CSV processing is fully offline.

---

## Limitations

- **Date range:** Slack exports up to 12 months of daily data. For longer periods you can load multiple exports sequentially — each load replaces the previous data.
- **Message counts:** The four channel-type message columns (`public`, `private`, `DMs`, `shared`) count human messages only and exclude app/bot traffic. The `Messages posted` column in the raw CSV is a cumulative lifetime total and is not used directly.
- **Stickiness:** Days where weekly active people equal monthly active people (often early in a workspace's life) correctly register as stickiness = 1.0.

---

## License

MIT — do whatever you like with it.
