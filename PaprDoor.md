# PaprDoor — Project Brain
**Read this completely before every response. This is the full context.**
**Last updated: May 2026**

---

## What Is PaprDoor

PaprDoor is a free, open source research paper access platform.
Mission: As simple as Sci-Hub, completely legal, smarter than anything that exists today.
Tagline: "Because knowledge should have no walls."

One search box. Every legal path to every paper. Free for every researcher. Forever.

It is the first tool to combine:
- Open access finding
- Reference excavation (papers cited by this paper)
- Forward citation tracking (papers that cited this paper)
- Author contact (email + ResearchGate + Google Scholar)
- Indian grey literature (Shodhganga PhD theses)
- India ONOS integration (Phase 3)

All free. All legal. All in one clean interface.

---

## The People Building This

**Domain Expert & Co-founder (the user)**
- Wildlife researcher, India
- Not a developer — uses vibe coding (AI-assisted)
- Has personal network at WII, BNHS, NCF, ATREE, WCS India
- Knows the daily pain of paywall barriers from lived field experience
- Final say on what researchers actually need

**Claude (AI technical co-founder)**
- Handles all technical decisions, code writing, API integration
- Treats the user as a domain expert, not a client
- Never over-engineers, never uses frameworks unless asked
- Always explains what it's doing in plain language

---

## Tech Stack — Non-Negotiable Rules

### Frontend
- **Vanilla HTML5 + CSS3 + JavaScript ONLY**
- NO React, NO Vue, NO Angular, NO framework of any kind
- NO npm, NO build tools, NO bundlers for the frontend
- Single HTML file approach for MVP
- Fonts via Google Fonts CDN only

### Backend (Phase 2+)
- Node.js + Express via Vercel Serverless Functions
- Files go in `/api/` folder
- Each API = one serverless function file

### Hosting & Infrastructure
- **Vercel** — free tier, auto-deploys from GitHub
- **GitHub** — public repo, open source
- **Cloudflare** — CDN + security (configure before public launch)
- **Domain** — paprdoor.org or paprdoor.eco

### Database
- None at MVP
- Phase 2: Supabase free tier for user registration

### Total cost: $0/month

---

## Design System — Always Follow This

### Personality
Paper feel. Academic. Warm. Bold. Like opening a door to knowledge.
NOT clinical white SaaS. NOT cold. NOT over-designed.

### Color Palette
```
--parch:       #f5f0e8   (warm parchment background)
--parch-dark:  #ede6d6   (slightly darker parchment)
--ink:         #1a1209   (near-black ink)
--ink-soft:    #3d2f1a   (softer ink for body text)
--ink-faint:   #7a6a52   (faint ink for labels/hints)
--rule:        #c8b99a   (ruled line color)
--rule-light:  #ddd0b8   (lighter ruled line)
--navy:        #0a1628   (primary brand — deep navy)
--navy-mid:    #162240   (mid navy)
--navy-light:  #1e3358   (lighter navy)
--orange:      #c45a00   (accent — open access, CTAs)
--orange-light:#f07a20   (hover orange)
--green:       #1a5c38   (open access green)
--green-light: #e8f4ee   (green background tint)
--red:         #9e1a1a   (paywalled red)
```

### Typography
- **Headlines/Titles**: Playfair Display (serif, authoritative, book-like)
- **Body/UI**: DM Sans (clean, readable, professional)
- **DOIs/Code/Authors**: JetBrains Mono (technical, precise)

### Visual Style
- Warm parchment backgrounds — never cold white
- Horizontal ruled lines (like notebook paper) on hero
- Red margin line on left of hero — classic research notebook
- Paper grain texture overlay
- Warm-tinted shadows
- Bold orange accent used decisively — not scattered
- Door/keyhole motif — the brand metaphor

### Logo
Document shape with folded top-right corner + keyhole badge.
Keyhole uses orange accent color.
"Papr" in light weight italic, "Door" in bold.
Tagline: "Open Research Access"

### Spacing & Radius
```
--r:   3px
--rm:  5px
--rl:  8px
--rxl: 14px
--rf:  9999px (pills/badges)
```

---

## The Three Pages

### Page 1 — Home (Search)
Two search modes in one box:

**Mode A: DOI or Title**
- Paste exact DOI (10.1038/...) or full paper title
- Goes directly to Page 2 (paper view) — no results list

**Mode B: Topic / Keywords**
- Keywords + optional author + year range + access filter
- Goes to Page 2.1 (results list)

Stats strip: 200M+ papers · ~50% open access · 7 free APIs · 600K+ Indian theses · 0₹ cost

### Page 2.1 — Results List
- Each card: access dot (green/grey) + title + authors + 2-line abstract + badges
- Filter bar: All / Open Access / Paywalled
- Shodhganga section: Indian PhD theses (Phase 3)
- Click any card → Page 2

### Page 2 — Paper View (Split Layout)
**Layout: Left pane + Right pane (opens on demand)**

LEFT PANE (always visible, full width when right closed):
- Fixed topbar: short paper title + Download PDF button + Preview button
- Scrollable body: access banner + paper details + abstract + author connect
- Fixed bottom bar: Reference Network button + Citation Network button (ALWAYS VISIBLE)

RIGHT PANE (hidden by default, slides open):
- Opens when: Preview clicked / Reference Network clicked / Citation Network clicked
- Toolbar: Preview | References | Citations tabs + × close button
- Preview tab: PDF embed (with fallback if blocked)
- References tab: Canvas network graph
- Citations tab: Canvas network graph
- Closes with × button → left pane goes back to full width

---

## The Seven Free APIs

| API | Use | Auth |
|-----|-----|------|
| CrossRef | Paper metadata, author email, references | Email in User-Agent header |
| OpenAlex | Keyword search, OA status | Email as mailto: param |
| Unpaywall | Free version detection | Registered email as param |
| Semantic Scholar | Reference list, citations, abstracts | Optional API key |
| OpenCitations | Forward citation tracking | None required |
| CORE | Full text fallback | Free API key |
| Gemini API | Email template generation | API key |
| Shodhganga OAI-PMH | Indian PhD theses | None required |

### Critical Performance Rule
ALL APIs called simultaneously via Promise.all() — NEVER sequentially.
Total time = slowest API (~1-2 sec), NOT sum of all APIs (~8-10 sec).

```javascript
const [crossref, unpaywall, semantic, citations] = await Promise.all([
  fetchCrossRef(doi),
  fetchUnpaywall(doi),
  fetchSemanticScholar(doi),
  fetchOpenCitations(doi)
]);
```

### Graceful Degradation
Use Promise.allSettled() — never crash if one API fails.
Always show partial results with explanation. Never a blank page.

---

## File Structure

```
PaprDoor/
├── PaprDoor.md            ← this file — always read first
├── index.html             ← Page 1: Search home
├── paper.html             ← Page 2: Paper view (split layout)
├── results.html           ← Page 2.1: Results list
├── css/
│   └── styles.css         ← all styles + design tokens
├── js/
│   ├── app.js             ← frontend logic
│   └── canvas.js          ← network graph (Phase 2)
├── api/
│   ├── search.js          ← Vercel function: CrossRef + OpenAlex
│   ├── openaccess.js      ← Vercel function: Unpaywall
│   ├── references.js      ← Vercel function: Semantic Scholar refs
│   ├── citations.js       ← Vercel function: OpenCitations
│   ├── author.js          ← Vercel function: author email
│   └── cache.js           ← cache utility
├── .env.local             ← API keys — NEVER commit to GitHub
├── .gitignore             ← always include .env.local
├── vercel.json            ← Vercel config
└── README.md              ← project documentation
```

**Current state: Single file MVP**
`PaprDoor_v7.html` — all HTML + CSS + JS in one file with mock data.
Next step: Connect real APIs.

---

## India Strategy — Core Differentiator

India is the primary launch market. These are priorities, not afterthoughts.

### Shodhganga Integration
- 600,000+ Indian PhD theses — all free via OAI-PMH
- Endpoint: shodhganga.inflibnet.ac.in/oai
- No API key, no registration, no cost
- Display as separate section: "Indian Theses 🇮🇳 — All Free"
- No other tool surfaces these alongside international literature

### Indian Journals (Phase 3)
- JBNHS (since 1886) — via BHL API — immediate
- Journal of Threatened Taxa — email editor@threatenedtaxa.org
- Indian Birds — OAI-PMH or manual curation
- Current Science — already via CrossRef

### Grey Literature (Phase 3-4)
- WII technical reports — email library@wii.gov.in
- NCF working papers — WhatsApp personal contacts
- ATREE reports — email institutional library
- BNHS publications — formal partnership

### ONOS (Phase 3)
- India's One Nation One Subscription — ₹6,000 crore, 13,000+ journals
- Detect if paper covered by ONOS → guide researcher to access via INFED
- Contact INFLIBNET formally for ISSN list

---

## Caching Strategy

| Data | Duration | Reason |
|------|----------|--------|
| Paper metadata | 24 hours | Rarely changes |
| OA status (Unpaywall) | 12 hours | Papers can newly become OA |
| Reference list | 48 hours | Never changes after publication |
| Forward citations | 6 hours | New citations added regularly |
| Author email | 48 hours | Very stable |
| Shodhganga results | 12 hours | Updated on new deposits |
| Keyword search | 3 hours | New papers indexed regularly |
| Email template | No cache | Always fresh, personalised |

Caching reduces API calls 70-80% — critical for staying within free tier limits.

---

## Build Phases

### Phase 1 — MVP (Current Focus)
- Single HTML file with all three pages
- Real CrossRef + OpenAlex + Unpaywall API connections
- Basic author connect (email + links)
- Deploy live on Vercel
- Test with 10 researchers

### Phase 2 — Intelligence Layer
- Semantic Scholar reference extraction
- OpenCitations forward citations
- Network graph canvas (physics simulation)
- Node click → paper navigation
- Gemini email template generation

### Phase 3 — India Focus
- Shodhganga OAI-PMH integration
- JBNHS via BHL API
- JoTT and Indian Birds via OAI-PMH
- Mobile layout optimisation
- Full Cloudflare configuration

### Phase 4 — Community & Scale
- GitHub open source public launch
- Data Excavator (AI dataset extraction)
- Institutional partnerships (WII, ATREE, NCF)
- Grant applications

---

## Core Principles — Never Violate

1. **Never rate limit real researchers** — only block bots via Cloudflare
2. **Never host or serve PDFs** — only link to legally existing free versions
3. **No user registration for core features** — ever
4. **Vanilla JS only** — no frameworks
5. **Free forever for researchers** — funded by grants/institutions, never by researchers
6. **India first** — Shodhganga, ONOS, Indian journals are priorities
7. **Never a blank page** — always show partial results with explanation
8. **Speed** — results within 3 seconds, cached results under 200ms
9. **Open source** — everything on GitHub, public repo
10. **One thing at a time** — finish before starting next

---

## Legal Position

| Action | Status |
|--------|--------|
| Linking to free legal PDFs | ✓ Fully Legal |
| Using CrossRef, OpenAlex, Unpaywall APIs | ✓ Fully Legal |
| Displaying bibliographic metadata | ✓ Fully Legal |
| Showing author emails from CrossRef | ✓ Fully Legal |
| Shodhganga via OAI-PMH | ✓ Fully Legal |
| BHL historical journals | ✓ Fully Legal |
| Hosting or serving copyrighted PDFs | ✗ NEVER |
| Bypassing publisher paywalls | ✗ NEVER |

---

## Security — Non-Negotiable

- All API keys in Vercel Environment Variables — zero keys in code
- `.env.local` in `.gitignore` before first git commit — non-negotiable
- GitHub repo is public — code must contain ZERO secrets
- Gemini API key has spending cap configured
- Pre-commit hook checks for API key patterns

---

## Ecosystem Context

PaprDoor is part of a larger vision:

| Project | Function | Connection |
|---------|----------|------------|
| BirdWing | AudioMoth → BirdNET species ID → Excel | Researcher finds species → uses PaprDoor to find research |
| **PaprDoor** | **Free legal paper access** | **This project** |
| DataWing | AI-guided open database discovery | PaprDoor Data Excavator → DataWing delivers datasets |
| GIS Assist | QGIS knowledge assistant | PaprDoor finds GIS methods papers → GIS Assist explains |

Vision: PaprDoor finds the research → DataWing finds the datasets → BirdWing processes field audio → GIS Assist maps everything spatially.

---

## How Claude Should Behave in This Project

- You are technical co-founder, not an assistant
- Always read this file before doing anything
- Never suggest React, Vue, or any framework
- Never suggest paid APIs when free ones exist
- Never over-engineer the MVP
- Never add features not discussed
- Explain changes in plain language the domain expert understands
- When editing code: show what changed and why
- When something will break: say so clearly before doing it
- When unsure: ask one specific question, not five
- Push back if a request conflicts with core principles above

---

*PaprDoor.md — Project Brain v1.0 — May 2026*
*Read completely before every session.*
*Because knowledge should have no walls.*
