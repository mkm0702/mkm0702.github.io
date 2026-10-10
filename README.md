# Manish Kumar Milan – portfolio site

A static site for GitHub Pages. No framework and no build step: `index.html` contains the HTML, CSS and JavaScript.

```
index.html               the whole site
404.html                 shown for unknown URLs
favicon.svg              tab icon (a real ArUco marker, id 13)
apple-touch-icon.png     iPhone home-screen icon
og-image.png             link preview for LinkedIn, WhatsApp, X
.nojekyll                tells GitHub Pages to serve the files as they are
assets/
  Manish_Kumar_Milan_Resume.pdf    the "Download résumé" file
  videos/                          project videos (see below)
```

## Add your videos

Put each file in `assets/videos/` with exactly this name. GitHub Pages is case-sensitive, so `PTM.mp4` will not be found.

| File                 | Shows in                                  |
|----------------------|-------------------------------------------|
| `ptm.mp4`            | Page-turning machine (featured)           |
| `hil-serl.mp4`       | Thesis: human-in-the-loop RL disassembly   |
| `bimanual-vla.mp4`   | Two-arm handover with SmolVLA             |
| `amr.mp4`            | Autonomous mobile robot                   |
| `code-as-policy.mp4` | Coding agent as a driving policy          |
| `world-models.mp4`   | World models from scratch                 |

Nothing in the HTML needs editing. A slot shows its video as soon as the file exists. Until then, project cards stay text-only and the machine slot shows "coming soon". Videos play muted and looped while on screen, pause when scrolled away, and have controls for sound and full screen. Portrait phone clips are letterboxed, not cropped.

**Compress first.** H.264 MP4 plays in every browser. Phones often record HEVC, which some browsers cannot play.

```bash
ffmpeg -i input.mov -vf scale=-2:720 -c:v libx264 -crf 26 -preset slow -pix_fmt yuv420p -movflags +faststart -an ptm.mp4
```

Remove `-an` to keep the sound. To add an optional poster frame:

```bash
ffmpeg -ss 2 -i ptm.mp4 -frames:v 1 -q:v 3 ptm.jpg
```

**Size limits.**

- GitHub rejects files over 100 MB.
- Uploads through the github.com website are limited to 25 MB per file. Push bigger files with git.
- A published Pages site can be at most 1 GB.

Aim for 5–20 MB per clip. Commit videos as normal files, not Git LFS: a site deployed from a branch serves the LFS pointer instead of the video.

**Long videos on YouTube or Google Drive.** In `index.html`, replace `data-video="assets/videos/ptm.mp4"` with one of these:

- `data-youtube="VIDEO_ID"`
- `data-drive="FILE_ID"`

The Drive file must be shared as "Anyone with the link", and it must be a single file, not a folder.

## Publish on GitHub Pages

1. On GitHub, create a **public** repository named exactly `mkm0702.github.io`.
2. Push the contents of this folder to the root of that repository:

   ```bash
   cd mkm0702.github.io
   git init
   git add .
   git commit -m "Portfolio site"
   git branch -M main
   git remote add origin https://github.com/mkm0702/mkm0702.github.io.git
   git push -u origin main
   ```

3. In the repository, go to **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch** and Branch to **main**, folder **/ (root)**, then click **Save**.
4. After a minute or two the site is live at **https://mkm0702.github.io**.

To update later, edit the files, then run `git add . && git commit -m "Update" && git push`. Pages redeploys on its own.

## Preview on your computer

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` by double-clicking also works, but the local server behaves exactly like the live site.

## Edit the text

Each section of `index.html` starts with a comment banner: HERO, FEATURED, PROJECTS, EXPERIENCE, EDUCATION, SKILLS, RECOGNITION, CONTACT. The colours are CSS variables at the top of the `<style>` block.

## Before you share the link

- Replace `assets/Manish_Kumar_Milan_Resume.pdf` with your corrected résumé, keeping the same file name.
- The World Models project says "Nov 2026 – present". If that is a typo, search for `Nov 2026` and fix it.
- When a project repository becomes public, uncomment the "Code on GitHub" line in that card.
- Point your résumé's "Website" link at https://mkm0702.github.io.
