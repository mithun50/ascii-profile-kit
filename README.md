# ascii-profile-kit

Build a clean, monochrome **animated GitHub profile**: an ASCII portrait that
types itself in like a terminal, a neofetch-style info panel, and a live
contribution graph that refreshes daily on its own — no paid services, no
tokens, no broken images.

This is my personal build (**Mithun Gowda B**), wired end to end. Fork it, swap
in your photo and details, and ship your own.

<div align="center">

<table>
<tr>
<td valign="top"><img src="./avi-ascii.svg" width="370" alt="ASCII portrait" /></td>
<td valign="top"><img src="./info-card.svg" width="490" alt="Experience, stack, highlights" /></td>
</tr>
</table>

<img src="./contrib-heatmap.svg" width="860" alt="GitHub contribution graph" />

</div>

---

## How it works

GitHub strips `<script>` from READMEs, but it DOES run **SMIL and CSS animations
inside an SVG** loaded as an `<img>`. So all the motion lives inside self-hosted
SVGs in the repo — nothing ever 404s or gets rate-limited.

Two things make the portrait look *clean* instead of noisy:
1. **Monochrome** — one light-gray color, never per-character rainbow.
2. **Background removed + local contrast (CLAHE)** — so the subject sits on blank
   space and the face has real highlights/shadows instead of being a dark blob.

## What's inside

```
PROMPT.md                    a paste-into-Claude-Code prompt that drives it all
profile-README-template.md   the README that goes on your profile
requirements-local.txt       deps for the one-time local image prep
scripts/
  prep_photo.py              rembg background removal + CLAHE contrast (run once)
  make_ascii_svg.py          photo  -> typing monochrome ASCII portrait
  make_info_card.py          your experience/stack -> neofetch info panel  <- EDIT
  fetch_contributions.py     scrapes your real contributions (no auth)
  render_heatmap_svg.py      contributions -> animated box graph
  requirements.txt           deps the daily workflow needs
.github/workflows/
  update-profile-art.yml     refreshes the graph every day, automatically
```

## Quickstart

```bash
# 0. deps
pip install -r requirements-local.txt        # local prep
pip install -r scripts/requirements.txt      # scraper

# 1. portrait  (STATIC=1 shows the final frame; drop it for the animated file)
python scripts/prep_photo.py path/to/your-photo.jpg source-prepped.png
python scripts/make_ascii_svg.py              # -> avi-ascii.svg

# 2. info panel  (edit the ROWS + HOST at the top of the script first)
python scripts/make_info_card.py              # -> info-card.svg

# 3. contribution graph
GH_PROFILE_USER=YOUR_USERNAME python scripts/fetch_contributions.py
python scripts/render_heatmap_svg.py          # -> contrib-heatmap.svg

# 4. README
cp profile-README-template.md README.md       # then fill in name / tagline / links
```

Create a **public repo named exactly your GitHub username**, drop in the three
SVGs, `README.md`, the `scripts/` folder, `data/contributions.json`, and
`.github/`, then push. Set **Settings → Actions → General → Workflow permissions
→ Read and write** and run the workflow once so the graph appears immediately.
After that it updates itself daily.

## Tuning cheatsheet

| Want to…                    | Where |
| --------------------------- | ----- |
| Punchier / lighter face     | `CONTRAST`, `GAMMA`, `WHITE_FLOOR` in `make_ascii_svg.py`; `clipLimit` in `prep_photo.py` |
| Type faster / slower        | `ROW_DUR`, `STAGGER` in `make_ascii_svg.py` |
| Change experience / stack   | `ROWS` and `HOST` in `make_info_card.py` |
| Info panel too tall         | bump `H` in `make_info_card.py`, then re-match `width=` in the README |
