---
name: publish-post
description: Publish one blog post that Alex has reviewed and explicitly approved with "publish". Builds the post page from the drafts template, updates the blog list, sitemap, llms.txt and feed, audits, then commits and pushes.
---

# Publish an approved blog post

**Precondition:** Alex said "publish" for this specific post in this conversation. If not, stop and ask.

1. Read `stoicscience-blog-drafts/drafts/<folder>/draft.md` (Alex's corrected version). Re-read it: he may have edited it.
2. Before building, record what Alex changed versus the team's version in `voice.md` → "Learned from Alex's edits".
3. Build `blog/<slug>.html` with the same converter as the preview:
   `python -I stoicscience-blog-drafts/tools/preview.py stoicscience-blog-drafts/drafts/<folder> --publish`
   (sections, [n] reference marks → References list, figures, tables; it stops on a mark with no reference).
   If the post has figures, copy the image files into `blog/images/` first.
4. The same command automatically rebuilds the Industry Insights list (`blog/index.html`, classic archive
   layout, newest first), `feed.xml`, the Industry Insights block of `sitemap.xml` and of `llms.txt`.
   Check its output line ("Rebuilt list, feed.xml, sitemap.xml, llms.txt for N post(s).").
7. Run `standards-auditor` on the new page and the blog list. Fix HIGH findings.
8. `git-operations`: stage by explicit path, commit ("Publish blog post: <title>"), push.
9. Mark the draft `published` and add a row to `history.md`. Mark the topic `used` in `topics.md`.
10. Tell Alex the live URL and that the LinkedIn version is the next step (when that work stream starts).
