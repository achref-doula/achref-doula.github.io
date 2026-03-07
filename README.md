# achref.d.gitlab.io — Personal Academic Website

Your personal academic website, ready to deploy on GitLab Pages.

---

## How to Publish (First Time Setup)

### Step 1: Create the GitLab Repository

1. Go to gitlab.com and sign in.
2. Click **New project** → **Create blank project**.
3. **Project name**: `achref.d.gitlab.io`
   - For your site to be at `https://achref.d.gitlab.io`, the repo MUST be named exactly `yourusername.gitlab.io`.
   - If you name it anything else (e.g. `my-website`), the URL becomes `https://yourusername.gitlab.io/my-website/`.
4. Visibility: **Public** (required for Pages on free tier).
5. Uncheck "Initialize repository with a README".
6. Click **Create project**.

### Step 2: Upload the Files

**Option A — GitLab Web Interface (easiest):**

1. On your repo page, click **Upload file** or use the **Web IDE** (Edit dropdown → Web IDE).
2. Upload ALL these files:
   - `index.html`
   - `.gitlab-ci.yml` (this is the key file for GitLab Pages)
   - `README.md`
   - `images/` folder (with your profile photo)
3. Commit to the `main` branch.

**Option B — Git Command Line:**

```bash
cd achref.d.gitlab.io
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://gitlab.com/achref.d/achref.d.gitlab.io.git
git push -u origin main
```

### Step 3: GitLab Pages Deploys Automatically

Unlike GitHub, there is no "enable Pages" toggle. GitLab reads `.gitlab-ci.yml` and deploys automatically.

1. After pushing, go to **Build → Pipelines** in the left sidebar.
2. Wait for the pipeline to pass (green checkmark, ~1 minute).
3. Go to **Deploy → Pages** to see your site URL.
4. Visit **https://achref.d.gitlab.io**

If the pipeline fails: make sure the file is named exactly `.gitlab-ci.yml` (with leading dot) and your default branch is `main`.

---

## How GitLab Pages Works

GitLab Pages uses a CI/CD pipeline defined in `.gitlab-ci.yml`. Here is what ours does:

```yaml
pages:
  stage: deploy
  script:
    - mkdir -p public
    - cp index.html public/
    - cp -r images public/
  artifacts:
    paths:
      - public
  only:
    - main
```

The key word is `pages:` — GitLab recognizes this as a Pages job. It takes whatever ends up in the `public/` folder and serves it as your website. Every push to `main` triggers this pipeline and updates the site.

---

## Add Your Profile Photo

1. Add your photo as `images/profile.jpg` (square, at least 400x400px).
2. Commit and push. Pipeline runs and deploys automatically.

---

## How to Update the Site

### Adding a New Publication

Find the publications section in `index.html` and copy-paste any `<li class="pub-item">` block:

```html
<li class="pub-item">
    <div class="pub-thumb-placeholder">XX</div>
    <div class="pub-info">
        <div class="pub-title">Your Paper Title</div>
        <div class="pub-authors"><span class="me">Achref Doula</span>, Co-Author</div>
        <div class="pub-venue">Conference 2026</div>
        <div class="pub-links">
            <a href="https://arxiv.org/abs/XXXX.XXXXX" target="_blank">arXiv</a>
            <a href="link-to-code" target="_blank">Code</a>
        </div>
    </div>
</li>
```

To add a thumbnail image: save a paper figure to `images/publication_images/` and replace the placeholder div with:
```html
<img src="images/publication_images/your_image.png" class="pub-thumb" alt="desc">
```

### Adding a News Item

Add at the top of `<ul class="highlights">`:

```html
<li><span class="date">[Mon, Year]</span> Your news here.</li>
```

### Updating Bio or Contact Info

Edit the `<div class="bio">` paragraphs or `<ul class="sidebar-info">` directly in `index.html`.

---

## Pushing Updates

**Option A — GitLab Web IDE (no Git needed):**
1. Go to your repo → Edit dropdown → Web IDE.
2. Edit `index.html` in the browser.
3. Source Control (left sidebar) → commit message → Commit to main.
4. Site updates in ~1 minute.

**Option B — Git Command Line:**
```bash
git add .
git commit -m "Added new publication"
git push
```

**Option C — Edit single file:**
1. Click `index.html` in repo → Edit → Edit single file.
2. Make changes → Commit changes.

---

## File Structure

```
achref.d.gitlab.io/
├── index.html              ← The entire website (edit this)
├── .gitlab-ci.yml          ← Deployment config (don't touch)
├── README.md               ← This file
└── images/
    ├── profile.jpg         ← Your photo (REPLACE THIS)
    └── publication_images/ ← Paper figures (optional)
```

---

## Tips

- **Preview locally**: Double-click `index.html` to view in browser before pushing.
- **Custom domain**: Deploy → Pages → New Domain → add your domain and follow DNS instructions.
- **Pipeline status**: Check Build → Pipelines after pushing to confirm deployment.
- **Google indexing**: Submit your URL at Google Search Console to speed up indexing.

## Troubleshooting

- **Pipeline fails**: Check `.gitlab-ci.yml` exists and branch is `main`.
- **404 error**: Repo must be public; check Deploy → Pages; wait 1-2 min.
- **Images broken**: Use relative paths (`images/profile.jpg` not `/images/profile.jpg`).
- **Old content showing**: Hard refresh with Ctrl+Shift+R.
