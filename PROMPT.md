# The Prompt

Copy everything in the block below into **Claude Code** (claude.com/claude-code)
opened in this template folder. Fill in the four bracketed lines first. That's it.

---

```
You are helping me build an animated GitHub profile README using the scripts in
this folder. Do NOT rewrite the scripts from scratch -- use them as-is and only
edit the marked config sections.

My details:
- GitHub username: [YOUR_USERNAME]
- Name + tagline: [YOUR NAME — Title · Title · Title]
- A photo of me to turn into ASCII art: [path/to/photo.jpg]
- Links (portfolio / linkedin / instagram): [paste them]

Here is exactly what I want you to do, in order:

1. Check my tools. I need Python 3 with: pillow, numpy, opencv-python, rembg,
   onnxruntime (for the local one-time image prep) and requests + beautifulsoup4
   (for the contribution scraper). Install anything missing from
   requirements-local.txt and scripts/requirements.txt.

2. Create a public repo named EXACTLY my username (so GitHub renders it on my
   profile). Use the gh CLI if I'm logged in; otherwise walk me through it.

3. Portrait: run `python scripts/prep_photo.py <my photo> source-prepped.png`
   (removes the background with rembg + boosts local contrast with CLAHE so my
   face is legible, not a dark blob), then `python scripts/make_ascii_svg.py` to
   produce avi-ascii.svg -- a clean MONOCHROME ascii portrait that "types" itself
   in like a terminal. Render a preview with `qlmanage -t -s 900 -o . avi-ascii.svg`
   (macOS) and show it to me. Tune CONTRAST / GAMMA / WHITE_FLOOR in the script
   (and clipLimit in prep_photo.py) until the face reads well. Note: the SVG
   starts blank at t=0 and reveals on load -- to preview the final frame, set
   STATIC=1 when generating.

4. Info panel: edit the ROWS list + HOST at the top of scripts/make_info_card.py
   with my real experience / stack / highlights (NOT github stats -- the graph
   covers those), then run it to produce info-card.svg. Keep it the same height
   as the portrait; if it overflows, bump H in the script.

5. Contribution graph: run `python scripts/fetch_contributions.py` (set
   GH_PROFILE_USER to my username) then `python scripts/render_heatmap_svg.py`
   to produce contrib-heatmap.svg -- a GitHub-style grid of boxes that reveal
   cell by cell, with a Less->More legend and my real streak stats.

6. README: copy profile-README-template.md to README.md and fill in my name,
   tagline, and links. The portrait (width 370) and info card (width 490) sit in
   a table so they're the same height.

7. Automation: copy .github/workflows/update-profile-art.yml in. It re-scrapes
   my contributions and re-renders the graph every day with zero auth. After the
   first push, set the repo's Settings -> Actions -> Workflow permissions to
   "Read and write", then trigger the workflow once so the graph exists
   immediately.

8. Commit and push everything. Show me the final rendered profile and the repo
   URL. Then tell me the one-line commands to re-tune the portrait or edit the
   info panel later.

Be visual: render previews as you go and iterate with me before pushing. Keep it
clean and monochrome -- no rainbow colors, no glitch effects.
```

---

That's the whole thing. The scripts do the heavy lifting; the prompt just drives
them and customizes the content to you.
