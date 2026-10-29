# Stoic Science website: orchestrator rules

This file is read automatically by every Claude Code session in this repo.
The main session is the **orchestrator**: it plans, talks to Alex, and routes
work to the four agents in `.claude/agents/`. It does not skip their reviews.

Team structure adapted from Jim Caton's AgentTeams (MIT licence),
https://github.com/jlcatonjr/agentteams, trimmed by hand for a small static site.

---

## Working rules (set by Alex, non-negotiable)

1. **Ask and confirm direction before writing or changing any file.**
2. **One small step at a time.** Finish, show, wait.
3. **Comment the code** so Alex can learn from it (HTML, CSS and JS).
4. **Keep explanations short.**
5. **Never commit or push without Alex saying "commit".** He reviews first.
6. **Recommend, don't list.** When asked "what do you recommend?", pick one and say why.
7. **Re-read a file before editing it.** Alex edits files himself between steps; never undo his changes.
8. **Never invent facts.** Anything not in the "Decisions" section below or in approved copy: ask Alex.

## Team and routing

| Agent | Use it for | Writes files? |
|---|---|---|
| `design-lead` | Layout, CSS, colour, type, spacing, responsive behaviour, page structure | Yes, only for the step Alex approved |
| `copy-editor` | Every visible word: tone, length, AI-sounding phrases, factual claims | Yes, copy only, for the approved step |
| `standards-auditor` | Final check of a page before Alex reviews it: HTML, accessibility, links, SEO and AI-ready layer | No (read-only, reports findings) |
| `git-operations` | Staging, commit, push, after Alex says "commit" | Git only |

**Model use (cost, Alex 28 Oct 2026):** agents run on the model set in their own file (Sonnet; git-operations on
Haiku). Call agents by name, not `general-purpose`. One-off research or review jobs with no agent file: pass
model: sonnet.

**The team does the work, not the orchestrator (Alex, 28 Oct 2026).** The main session (Opus 5.5, medium effort)
plans, talks with Alex, decides and briefs; it does **not** write site files itself. Every build or edit of
HTML, CSS, JS, sitemap, llms.txt or images goes to `design-lead`; wording to `copy-editor`; checks to
`standards-auditor`; git to `git-operations`. The orchestrator writes only the brief (which file, what change,
which rules apply) and reads the agent's short report. Exceptions: CLAUDE.md, the tracker and dashboard
`data.js` (record-keeping), and a one-line fix Alex asks for directly. No monthly upkeep routine (Alex declined).

**Standard flow for any page or change:**
1. Orchestrator states the step in one or two lines and waits for Alex's go.
2. `design-lead` builds or changes the markup/CSS.
3. `copy-editor` checks the words (and flags any claim not on the approved list).
4. `standards-auditor` checks the result; HIGH findings are fixed before showing Alex.
5. Orchestrator shows Alex what changed (short) and how to preview it.
6. On "commit": `git-operations` commits and pushes.

## Blog department (added 25 Oct 2026)

Goal: **one post per week**, current with industry news, in Alex's voice, nothing published without Alex.

| Agent | Job | Writes |
|---|---|---|
| `news-scout` | Scours the web for verified industry news; keeps the dynamic topic backlog fresh and rotating | `topics.md` only |
| `blog-writer` | Picks a topic (news preferred, category rotates), drafts 350–600 words in Alex's voice, links every source | the post's `draft.md` |
| `copyright-editor` | Checks copied wording, quotes, attribution and links; tightens to the shortest insightful version | `draft.md`, `review.md` |
| `copy-editor` | (existing) claims, site wording rules, confidentiality | notes in `review.md` |
| `human-voice-scorer` | Quality gate, voice weighted heaviest: **≥ 80/100 and voice ≥ 26/35** to reach Alex | `review.md` only |

- **Drafts live only in `C:\PROIECTE\StoicScience\stoicscience-blog-drafts\`** (local, no git, never uploaded).
  Never write a draft into this repo: it is public and Pages publishes everything in it.
- `voice.md` in the drafts folder is the voice reference. **Standing rule (Alex, 25 Oct 2026): keep it
  continuously up to date.** After every correction Alex makes to a draft, and after every long prompt in
  which his way of writing or thinking shows, add a dated line to "Learned from Alex's edits and prompts"
  and refine the rules above it. The goal is that the writers truly synthesize his voice.
- **Publishing needs Alex's explicit "publish"** for that post (that word is also the commit go for that post).
  Then: fill `templates/post.html` → `blog/<slug>.html`, add the post to the top of `blog/index.html`,
  add it to `sitemap.xml`, `llms.txt` and `feed.xml`, run `standards-auditor`, then `git-operations` pushes.
  Log it in the drafts `history.md`.
- Commands: `/weekly-blog` runs steps 1–6 and hands Alex the draft; `/publish-post <slug>` publishes an approved one.
- **Article format (25 Oct 2026):** sections Introduction / <middle title chosen per subject, e.g. What happened> / Takeaway; optional figures and tables;
  journal layout (full-width justified text, first-line indent, centred headings);
  **reference marks [n] (superscript) instead of in-text links**, linking to a numbered References list that
  links to the sources. Converter and preview: `stoicscience-blog-drafts/tools/preview.py`.
- **Industry & Market (IND) posts:** no company names for layoffs/hiring (aggregate by type); add a lag note right
  after any statement based on lagging official data; charts drawn in-house from public data (SVG, navy/gold).
- **Header:** Industry Insights sits on its own between the wordmark and Home (gold pill with a live dot).
- The Blog nav link and `blog/index.html` stay **uncommitted until the first post is published**.

## Business dashboard (standing rule, Alex, 25 Oct 2026)

`C:\PROIECTE\StoicScience\dashboard\` is a local, explorable map of the whole business. `index.html` draws it (2D map; the 3D globe trial was rejected as too noisy on 25 Oct 2026)
and `data.js` holds the facts. **Whenever work moves (a task finished, a status changed, a new item or a
decision), update `data.js` in the same turn**: the node's status, done/next/blockers, the `nextMove` banner
and the `updated` date. Alex should never need to ask for a summary; the dashboard is the live view.
**Articles on the map (Alex, 27 Oct 2026):** show only the article currently in progress, numbered ("Post #n");
remove it from the map once published and keep the published count on the Industry Insights card. In replies,
mention only the active article.

## SOP department

Private. Its design, rules, effort settings and the backup plan live in `C:\PROIECTE\StoicScience\CLAUDE.md` (outside this public repo; backed up to Drive and the private repo). Read it before any SOP or backup work. Never copy that content back into this file.

## Technical ground rules

- **Plain hand-written HTML, CSS and a little vanilla JS.** No frameworks, no build step, no npm packages, no CSS/JS from CDNs. GitHub Pages serves `main` / root as-is.
- **Fonts are self-hosted** in the repo (open licence). No Google Fonts requests.
- **Relative links only** (`about.html`, `css/style.css`) in normal pages. Exception: `404.html` and
  `home/index.html` use root links (`/css/style.css`) because they can be served at any depth.
- **Live at `https://stoicscience.us`** since 25 Oct 2026 (GitHub Pages, custom domain). The `CNAME` file
  in the repo root holds the domain: **never delete it**. The github.io address now redirects to the domain.
- `home/index.html` redirects the old Google Sites address `/home` to `/`.
- **CSS cache-busting:** stylesheet link carries a version tag (`style.css?v=1`); bump it on every CSS change.
- `.nojekyll` at the root so GitHub Pages serves files untouched.
- Approved third-party services (Alex): **Web3Forms** (contact form, same as xp-site) and **GoatCounter**
  (visitor statistics, cookieless, site code `alexstoicu`, added 27 October 2026). The GoatCounter tag goes in the
  `<head>` of every public page and the post template; local previews strip it so they don't count.
- Social/Open Graph image must be PNG or JPG, not SVG.

## Decisions (approved by Alex, 25 Oct 2026)

**Positioning and tone**
- Clean, crisp, professional, corporate. Navy + gold, restrained. Nothing retro.
- Short and scannable. The site hooks; a future blog does the depth.
- Do not lead with credentials. **No "M.Sc." anywhere on the site.**
- Confident, not salesy. Precision over persuasion.
- The practice is on hold, not closed: credible and complete, **no urgency or "taking clients now" language**.
- **No "LLC" anywhere** (not filed yet). Footer: `© 2026 Stoic Science`.
- Location: "Research Triangle Park, NC & Surrounding Area" (not Durham-Raleigh). Hero sub-line:
  `Alex Stoicu | Research Triangle Park, NC & Surrounding Area | Available Remotely Worldwide`.
- Say "pharmaceutical development pipeline", not "drug development".
- Audience wording for meta/structured data: "pharmaceutical, radiopharmaceutical and early-stage biotech teams".

**Brand**
- Name: Stoic Science. Tagline: `Precision | Discipline | Compliance` (design element, not links, until the blog exists).
- Gold accent `#C8A96E`. Navy `#1B2A41`. Font: **Inter** throughout (self-hosted, 400/600/700). "Modern" direction, chosen 25 Oct 2026.
- Logo: gold text wordmark for now.
- **Alignment rule (site-wide):** centered layout, left-aligned reading text. Headings, intros, short card
  text, labels and buttons are centered on the hero's axis; bullet lists and long-form text (About story,
  blog posts) are left-aligned inside a centered column. Exception chosen by Alex: engagement-model cards
  keep left-aligned text under a centered section heading.

**Pages (nav order):** Home, About, Services (shown as "What I Offer"), Sample Work, Contact.
Blog later: a professional blog about real-world radiopharmaceutical situations.

**Services (decided 25 Oct 2026)**
- Five cards: GxP Documentation, Method Validation & Transfer, Radiopharmaceutical Compliance,
  Instrument Qualification (for smaller labs without their own qualification program), GxP Training.
- **Not offered:** gap assessment, inspection readiness. Don't add them.
- Method Validation uses **ICH Q2(R2)**; techniques: HPLC-RAD, radio-TLC, GC-FID, headspace GC-FID, wet chemistry,
  raw material and API testing. **Not listed anywhere:** UV-Vis, FTIR, Karl Fischer, osmolality, particle sizing, HPLC-MS.
- Say **GxP**, never GMP (exception: the regulation name "EU GMP Annex 15").
- Alex's radiochemistry lab role is **current** (present tense: "I run the lab").
- "Standards I work to": 21 CFR 211, 21 CFR 212, USP <823>, ICH Q2(R2), EU GMP Annex 15.
- A "Typical situations" block on the Services page.
- Market research (25 Oct 2026): nobody in this market publishes prices or real sample documents;
  avoid "global leader", "trusted partner", "end-to-end", "comprehensive solutions", "guaranteed", exclamation marks.

**Sample Work codes:** `STO-SOP-RCP-001` Radiochemical Purity Testing (the one real entry),
`STO-SOP-DEV-001` Laboratory Deviation & Investigation (coming soon),
`STO-SOP-PRM-001` Primary Reference Material (coming soon).
**Sample viewer rule (Alex, 28 Oct 2026; replaces "downloadable PDF"):** samples are **view only**. An approved,
redacted PDF is converted locally into watermarked page images (`C:\PROIECTE\StoicScience	ools\sop_pages.py`,
watermark "SAMPLE · Stoic Science") and shown in a lightbox viewer (dimmed, blurred background; no text layer, no
download or print, right-click/drag/copy blocked). **The PDF itself is never published.** Nothing can make a public
page fully copy-proof (screenshots); the watermark and redaction cover that. Viewer code and a local demo using the
unreviewed RCP-001 (watermark "DEMO · NOT FOR PUBLICATION") live in `C:\PROIECTE\StoicScience\sop-viewer-demo\`,
outside the repo; they move into the site only when the first redacted SOP is approved.
Plan (25 Oct 2026): Alex will rewrite the SOPs as heavily redacted mock documents, downloadable as PDF;
readable parts limited to public guidance (USP, ICH). Document descriptions on the page are placeholders
until then. The gold "Please note" callout (mock SOPs, client IP under NDA) is approved and stays.

**Contact**
- Web3Forms form posting to `https://api.web3forms.com/submit`, reusing Alex's existing access key.
- Hidden fixed subject starting `Stoic Science inquiry` (his Gmail filter files it under the label **Stoic Science**).
- Hidden `botcheck` spam trap, fetch() script so the visitor stays on the page.
- LinkedIn: `https://www.linkedin.com/in/alexstoicu` (opens in a new tab).
- Public email address: not decided yet. Ask.

**Keep separate:** never link to or borrow style from Night Lab XP (`xp.stoicscience.us`) unless Alex asks.

## Open questions (ask when the step needs them, not all at once)

- About and Sample Work entry copy (live site gave only summaries).
- AI crawlers: allow training crawlers, or only search/answer crawlers?
- Card icons and the SAMPLE PDF: Alex drops them in the repo.
- Public email: Gmail or `alex.stoicu@stoicscience.us` (test the alias first).

## Domain switch (last step, only on Alex's go)

DNS is on Namecheap. Until the switch, GitHub Pages runs without a custom domain.
At switch time: set custom domain `stoicscience.us` in Pages, add GitHub's A/AAAA records for `@`,
point `www` CNAME to `alexstoicu.github.io`, **leave MX, SPF TXT and the `xp` CNAME untouched**,
then Enforce HTTPS. Re-check GitHub's IPs against their docs at that time.
