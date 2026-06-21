# Jestery's Portfolio Website

A digital artist & game designer portfolio website, configured for GitHub Pages deployment.

## File Structure

```
Jesterday Portfolio Website/
├── .nojekyll              # Tells GitHub Pages to skip Jekyll processing
├── 404.html               # Custom 404 page
├── README.md              # This file
├── index.html             # Home page
├── about.html             # About Me page
├── my-work.html           # Project gallery
├── analogue.html          # Analogue (board) games
├── digital.html           # Digital games
├── other.html             # Art & design work
├── contact.html           # Contact form
├── nav.html               # Navigation bar (loaded dynamically via JS)
├── CSS/
│   └── style.css          # All styling
├── JavaScript/
│   └── main.js            # Nav loading, theme toggle, lightbox, etc.
└── Assets/                # (You must add this folder - see below)
    ├── profpic.png
    ├── pick.png
    ├── king.png, queen.png, jack.png
    ├── girl.png, contact.png
    ├── aboutpic.png, skills.png, experience.png, education.png
    ├── anim.png, chardes.png, gameui.png
    ├── AnaGames/          # Analogue game images (hc1.jpg, madlab1.jpg, wrek1.jpg, ...)
    ├── DigGames/          # Digital game images (m1.png, p1.jpeg, ...)
    ├── animation/         # Animation videos & thumbnails (spongevid.mp4, spongethumb.png, ...)
    ├── Character Design/  # Character concept images (c1.png, c2.jpg, ...)
    └── Game UI/           # UI mockup images (ui1.png, ui2.png, ...)
```

> **Important:** The `Assets/` folder is **not included** in this package - you must copy your existing `Assets/` folder into the website root before deploying. All HTML/CSS/JS paths already point to `Assets/...` (relative), so just drop the folder in.

## What Was Fixed for GitHub Pages

This version has been patched so it works correctly on GitHub Pages **project sites**
(i.e. URLs of the form `https://<username>.github.io/<repo-name>/`).

### Fixes Applied

1. **`index.html` was corrupted** - contained stray Markdown code-fence markers (` ``` `)
   that broke the HTML structure. These have been removed and the file rebuilt as
   valid HTML.

2. **Wrong CSS path in `index.html`** - was `href="style.css"`, fixed to
   `href="CSS/style.css"`.

3. **Wrong page links in `index.html`** - was `href="Pages/analogue.html"` (the `Pages/`
   folder doesn't exist), fixed to `href="analogue.html"`.

4. **Absolute paths converted to relative in HTML** - all leading-slash paths like
   `/Assets/...`, `/JavaScript/main.js`, `/about.html`, `/nav.html` have been changed
   to relative paths (`Assets/...`, `JavaScript/main.js`, `about.html`, `nav.html`).
   Absolute paths resolve to the domain root, which breaks on project sites where the
   site lives under `/<repo-name>/`.

5. **Absolute paths converted to relative in CSS (`style.css`)** - 5 `background-image:
   url("/Assets/...")` declarations were also using absolute paths and silently failing
   on project sites, causing backgrounds to appear missing. The affected images are:
   - `Assets/blog.jpg` - used on `.page-content` (the parchment background on every
     page) and `.nav-btn` (the nav buttons)
   - `Assets/blackBG.jpg` - used on `body.dark-mode .page-content` and
     `body.dark-mode .nav-btn`
   - `Assets/border suit.png` - used on `.section-break` (the decorative divider
     between sections) in both light and dark modes

6. **`main.js` `fetch()` path fixed** - `fetch("/nav.html")` changed to
   `fetch("nav.html")` so the navigation bar loads correctly on project sites.

7. **Active-nav detection fixed** - the original code compared
   `button.dataset.page === window.location.pathname`, but on a project site
   `window.location.pathname` is `/repo-name/about.html` while `data-page` is
   `about.html` - so the active state never highlighted. Fixed by extracting just
   the filename from the pathname before comparing.

8. **`nav.html` `data-page` attributes fixed** - all `data-page="/index.html"` etc.
   changed to `data-page="index.html"` (relative) so nav clicks navigate correctly.

9. **Typo fixed in `other.html`** - `data-video="/Assets/animationcatvid.mp4"`
   (missing slash) corrected to `data-video="Assets/animation/catvid.mp4"`.

10. **`.nojekyll` added** - tells GitHub Pages to skip Jekyll processing so all
   asset folders (including ones with uppercase names like `CSS/`, `JavaScript/`,
   and `Character Design/`) are served as-is.

11. **`404.html` added** - GitHub Pages will serve this for any not-found URL,
    giving visitors a friendly link back to the home page.

## How to Deploy on GitHub Pages

### Step 1 - Create the repository

1. Go to <https://github.com/new> and create a **new public repository**.
   - Name it whatever you like, e.g. `portfolio` or `jesterday-portfolio`.
   - Do **not** initialize with a README (you'll add files manually).

### Step 2 - Upload the website files

**Option A - Web upload (easiest):**
1. Unzip this package locally.
2. Add your `Assets/` folder (with all subfolders) into the unzipped directory.
3. On your GitHub repo page, click **Add file → Upload files**.
4. Drag **all** files and folders (including `.nojekyll` - it's a hidden file, so
   make sure your file browser shows hidden files) into the upload area.
5. Commit with a message like "Initial portfolio upload".

**Option B - Git command line:**
```bash
# Clone the empty repo
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# Copy all the files from the unzipped package into this folder
# (don't forget to add your Assets/ folder too!)
cp -r /path/to/unzipped/website/* .
cp -r /path/to/unzipped/website/.nojekyll .

# Commit & push
git add .
git commit -m "Initial portfolio upload"
git push origin main      # or 'master' if that's your default branch
```

### Step 3 - Enable GitHub Pages

1. In your repository, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, select **Deploy from a branch**.
3. Under **Branch**, select **main** (or **master**) and the **/ (root)** folder.
4. Click **Save**.

### Step 4 - Wait & visit

- GitHub Pages builds take 1-5 minutes for the first deploy.
- Once built, your site will be live at:
  `https://<your-username>.github.io/<repo-name>/`
- You can check build status under the **Actions** tab of your repo.

### Optional - Custom domain

If you want to use your own domain (e.g. `jesterday.art`):
1. In **Settings → Pages → Custom domain**, enter your domain and click **Save**.
2. At your DNS provider, add a CNAME record pointing to
   `<your-username>.github.io.` (note the trailing dot).
3. Wait for DNS to propagate (can take up to 48 hours).
4. Once verified, tick **Enforce HTTPS** in the same settings panel.

> **Note on path strategy:** If you ever switch from a *project* site to a *user/org*
> site (i.e. the repo is named `<your-username>.github.io`), the site will be served
> from the domain root. The relative paths used in this version will **still work**
> in that case, so no changes are needed.

## Local Preview

To preview the site locally before pushing:

```bash
cd "Jesterday Portfolio Website"
python3 -m http.server 8000
```

Then open <http://localhost:8000/> in your browser.

> Local preview via `file://` will **not** work for the navigation bar, because
> `fetch("nav.html")` is blocked by the browser's CORS policy on the file protocol.
> Always use a local HTTP server (or just deploy to GitHub Pages and test there).

## Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| Nav bar missing | `.nojekyll` not uploaded, or `nav.html` missing | Re-upload both files (`.nojekyll` is hidden - enable showing hidden files) |
| Images broken | `Assets/` folder not uploaded, or filenames/paths don't match | Verify folder names match exactly (case-sensitive!), including `AnaGames/`, `DigGames/`, `Character Design/`, `Game UI/`, `animation/` |
| Page / nav-button background missing (parchment look gone, plain background instead) | `Assets/blog.jpg`, `Assets/blackBG.jpg`, or `Assets/border suit.png` not uploaded, OR you're running an old version of `style.css` that still had absolute `url("/Assets/...")` paths | Ensure those 3 images exist in `Assets/`, and ensure you're using the patched `style.css` from this package (run a hard-refresh: Ctrl+Shift+R / Cmd+Shift+R). Open DevTools → Network tab to confirm `blog.jpg` returns 200, not 404 |
| Section divider line between sections missing | Same as above - `Assets/border suit.png` not loaded | Check `Assets/border suit.png` exists (note the space in the filename) |
| Active nav page not highlighted | Cached old `main.js` | Hard-refresh (Ctrl+Shift+R / Cmd+Shift+R) |
| 404 on every page | Wrong branch selected in Pages settings | Settings → Pages → Branch → ensure correct branch & `/ (root)` folder |
| Site builds but CSS missing | `CSS/` folder not uploaded, or got renamed to lowercase `css/` | Folder name is **uppercase** `CSS/` - case matters on GitHub Pages |

## Credits

- Design & content: Jessica Day
- Site fixed for GitHub Pages deployment: June 2026
