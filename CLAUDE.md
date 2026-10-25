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

**Standard flow for any page or change:**
1. Orchestrator states the step in one or two lines and waits for Alex's go.
2. `design-lead` builds or changes the markup/CSS.
3. `copy-editor` checks the words (and flags any claim not on the approved list).
4. `standards-auditor` checks the result; HIGH findings are fixed before showing Alex.
5. Orchestrator shows Alex what changed (short) and how to preview it.
6. On "commit": `git-operations` commits and pushes.

## Technical ground rules

- **Plain hand-written HTML, CSS and a little vanilla JS.** No frameworks, no build step, no npm packages, no CSS/JS from CDNs. GitHub Pages serves `main` / root as-is.
- **Fonts are self-hosted** in the repo (open licence). No Google Fonts requests.
- **Relative links only** (`about.html`, `css/style.css`), so the site works both at
  `https://alexstoicu.github.io/stoicscience-site/` (preview) and at `https://stoicscience.us`.
- **CSS cache-busting:** stylesheet link carries a version tag (`style.css?v=1`); bump it on every CSS change.
- `.nojekyll` at the root so GitHub Pages serves files untouched.
- The only third-party service is **Web3Forms** for the contact form (Alex approved it, same as xp-site).
- Social/Open Graph image must be PNG or JPG, not SVG.

## Decisions (approved by Alex, 25 Oct 2026)

**Positioning and tone**
- Clean, crisp, professional, corporate. Navy + gold, restrained. Nothing retro.
- Short and scannable. The site hooks; a future blog does the depth.
- Do not lead with credentials. **No "M.Sc." anywhere on the site.**
- Confident, not salesy. Precision over persuasion.
- The practice is on hold, not closed: credible and complete, **no urgency or "taking clients now" language**.
- **No "LLC" anywhere** (not filed yet). Footer: `© 2026 Stoic Science`.

**Brand**
- Name: Stoic Science. Tagline: `Precision | Discipline | Compliance` (design element, not links, until the blog exists).
- Gold accent `#C8A96E`. Navy hex and web font: **not yet chosen** (design-lead proposes, Alex confirms).
- Logo: gold text wordmark for now.

**Pages (nav order):** Home, About, Services (shown as "What I Offer"), Sample Work, Contact. Blog later.

**Sample Work codes:** `STO-SOP-RCP-001` Radiochemical Purity Testing (the one real entry),
`STO-SOP-DEV-001` Laboratory Deviation & Investigation (coming soon),
`STO-SOP-PRM-001` Primary Reference Material (coming soon).
Only the **watermarked SAMPLE PDF** is ever published, and only after it has been read and checked.

**Contact**
- Web3Forms form posting to `https://api.web3forms.com/submit`, reusing Alex's existing access key.
- Hidden fixed subject starting `Stoic Science inquiry` (his Gmail filter files it under the label **Stoic Science**).
- Hidden `botcheck` spam trap, fetch() script so the visitor stays on the page.
- LinkedIn: `https://www.linkedin.com/in/alexstoicu` (opens in a new tab).
- Public email address: not decided yet. Ask.

**Keep separate:** never link to or borrow style from Night Lab XP (`xp.stoicscience.us`) unless Alex asks.

## Open questions (ask when the step needs them, not all at once)

- Navy hex and web font.
- Method Validation card: broadened wording (UV-Vis, FTIR, Karl Fischer, osmolality, particle sizing), and ICH Q2(R1) vs Q2(R2) (R2 superseded R1 in 2023; flag, don't change silently).
- Instrument Qualification and GxP Training as extra service cards?
- About and Sample Work entry copy (live site gave only summaries).
- AI crawlers: allow training crawlers, or only search/answer crawlers?
- Card icons and the SAMPLE PDF: Alex drops them in the repo.
- Public email: Gmail or `alex.stoicu@stoicscience.us` (test the alias first).

## Domain switch (last step, only on Alex's go)

DNS is on Namecheap. Until the switch, GitHub Pages runs without a custom domain.
At switch time: set custom domain `stoicscience.us` in Pages, add GitHub's A/AAAA records for `@`,
point `www` CNAME to `alexstoicu.github.io`, **leave MX, SPF TXT and the `xp` CNAME untouched**,
then Enforce HTTPS. Re-check GitHub's IPs against their docs at that time.
