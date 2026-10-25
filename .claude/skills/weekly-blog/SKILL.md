---
name: weekly-blog
description: Run the weekly Stoic Science blog cycle (scout news, choose topic, draft, copyright and brevity check, claims check, human-voice score) and hand the passing draft to Alex for review. Never publishes.
---

# Weekly blog cycle

Follow `CLAUDE.md` → "Blog department". Drafts folder: `C:\PROIECTE\StoicScience\stoicscience-blog-drafts\`.
Nothing in this cycle touches the website repo, and nothing is published.

1. **Scout.** Run `news-scout`: refresh `topics.md` (verified sources only), report the top 3 candidates.
2. **Choose.** If Alex named a topic, use it. Otherwise `blog-writer` picks: industry news preferred, a different
   category from last week (`history.md`), no repeat within 6 months. Tell Alex the pick in one line.
3. **Draft.** `blog-writer` writes `drafts/<YYYY-MM-DD>-<slug>/draft.md` (front matter + 350–600 words).
4. **Rights and brevity.** `copyright-editor` checks and tightens; results appended to `review.md`.
   If BLOCKED, back to step 3.
5. **Claims.** `copy-editor` checks site wording rules, facts against sources, confidentiality (no employer details).
6. **Score.** `human-voice-scorer` scores it. PASS = total ≥ 80 **and** voice ≥ 26/35. Otherwise back to step 3
   with the notes. Maximum 3 rounds; after that, hand it to Alex with the score and say it didn't pass.
7. **Hand over.** Set the draft's status to `in-review`. Show Alex: title, summary, word count, score card,
   sources, and the full text. Ask him to review and correct. **Do not publish.**

When the agents aren't loaded as subagents (new session needed), run each step with a general-purpose agent
told to act as the agent defined in `.claude/agents/<name>.md`.
