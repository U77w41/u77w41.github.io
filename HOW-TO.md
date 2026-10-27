# How to maintain this site

Step-by-step guides for the common jobs. Every command is run in Terminal from the repo folder:

```bash
cd ~/uc/projects/u77w41.github.io
```

**Contents**

1. [One-time setup](#1-one-time-setup)
2. [Preview and publish (used by every guide below)](#2-preview-and-publish)
3. [Create a new blog post / project](#3-create-a-new-blog-post--project)
4. [Add a new item to the menu](#4-add-a-new-item-to-the-menu)
5. [Update the theme](#5-update-the-theme)
6. [Change the favicon or the Support link](#6-change-the-favicon-or-the-support-link)
7. [When something goes wrong](#7-when-something-goes-wrong)

---

## 1. One-time setup

1. Install Hugo and Go (Hugo uses Go to download the theme):

   ```bash
   brew install hugo go
   ```

2. Check the Hugo version. It must say `+extended` and be at least the version in `.github/workflows/deploy.yml` (`HUGO_VERSION`, currently 0.168.0):

   ```bash
   hugo version
   ```

3. Log in to GitHub once so `git push` works (pick **HTTPS** and **Login with a web browser**):

   ```bash
   brew install gh
   gh auth login --scopes workflow
   ```

---

## 2. Preview and publish

**Preview on your Mac** (nothing goes online):

```bash
hugo server -D
```

Open <http://localhost:1313>. The page reloads every time you save a file. `-D` also shows drafts. Press `Ctrl + C` to stop.

**Publish** (the site updates about 1 minute after the push):

```bash
git add -A
git commit -m "Short description of what you changed"
git push
```

Check progress in the [Actions tab](https://github.com/U77w41/u77w41.github.io/actions): a green tick means it's live.

---

## 3. Create a new blog post / project

Everything in `content/post/` appears as a card on the home page (the **Projects** page).

1. **Create the post.** Use dashes instead of spaces in the name:

   ```bash
   hugo new content post/My-Post-Name
   ```

   This creates `content/post/My-Post-Name/index.md` from the template in `archetypes/post/index.md`.

2. **Fill in the top section (front matter)** of `index.md`:

   | Field | What it does |
   |---|---|
   | `title` | Title on the card and page |
   | `slug` | Web address: `u77w41.github.io/p/<slug>/`. Don't change it after publishing, or old links break |
   | `description` | One sentence shown under the title on the card |
   | `date` | Publish date shown on the card |
   | `image` | Cover image file name, in the same folder (delete the line for no cover) |
   | `weight` | Position on the home page: 1 = first. Posts with the same weight are sorted newest first. To put a new post at the top, give it `1` and add 1 to every other post's weight (`weight: 0` doesn't work, Hugo treats it as "no weight" and puts the post last) |
   | `categories` | The coloured badge on the card, e.g. `Machine Learning`, `Power BI`, `Open Source` |
   | `tags` | Keywords shown at the bottom of the post |
   | `draft` | `true` = hidden from the live site. Change to `false` to publish |
   | `math` | Add `math: true` to use LaTeX (`$x^2$` inline, `$$ ... $$` for blocks) |

3. **Add images** to the same folder as `index.md`:
   - Cover: a landscape image (around 1200 × 600 px, under 500 KB) named exactly as in `image:`.
   - Inside the text: `![Caption text](chart.png)`

4. **Write the post** below the second `---` line, in normal Markdown. `##` headings automatically build the table of contents on the right.

5. **Preview** with `hugo server -D` and check the card on the home page and the post itself.

6. **Publish:** set `draft: false`, then commit and push (section 2).

**Change or remove a post later**

- Edit: change `index.md` and push.
- Remove: `git rm -r content/post/My-Post-Name`, then commit and push.
- If you must change a published `slug`, keep the old address working by adding it under `aliases:` (see `content/post/NSEScraper/index.md` for an example).

### Optional: a separate "Blog" section

Use this if you want articles listed separately from projects (at `u77w41.github.io/blog/`).

1. Create `content/blog/_index.md` with:

   ```yaml
   ---
   title: Blog
   description: Notes and write-ups
   menu:
       main:
           weight: 5
           params:
               icon: messages
   ---
   ```

2. Add this line to `config/_default/permalinks.toml`:

   ```toml
   blog = "/blog/:slug/"
   ```

3. Create articles with the same template as projects:

   ```bash
   hugo new content --kind post blog/My-Article
   ```

4. Blog articles appear only on the Blog page. To also show them on the home page, change `mainSections` in `config/_default/params.toml` to `["post", "blog"]`.

---

## 4. Add a new item to the menu

The left-hand menu is built from three kinds of entries. `weight` sets the order (smaller = higher).

### A. A new page (for example "About")

1. Create the folder and file `content/page/About/index.md`:

   ```yaml
   ---
   title: About
   slug: about
   menu:
       main:
           weight: 5
           params:
               icon: user
   ---

   Write the page content here in Markdown.
   ```

2. Preview, then commit and push. The page is at `u77w41.github.io/about/`.

Existing menu weights: Projects 1, Resume 2, Publications 3, Certificates 4, Support my work 100 (kept last).

### B. A link to another website

Add this to the end of `config/_default/menu.toml`:

```toml
[[main]]
    identifier = "kaggle-link"     # any unique name
    name       = "Kaggle"          # text shown in the menu
    url        = "https://www.kaggle.com/ujjwalchowdhury"
    weight     = 6
    [main.params]
        icon   = "link"
        newTab = true              # open in a new browser tab
```

### C. A social icon (the small icons under your name)

Copy one of the `[[social]]` blocks in `config/_default/menu.toml` and change `identifier`, `name`, `url`, `weight` and `icon`.

### D. A tool in the Tools dropdown

The four-circle icon with the small ▾ arrow, after Email, opens a list of your web tools. Each tool is a `[[social]]` block with `parent = "tools"`. To add one, copy the ANURUP block at the end of the social section in `config/_default/menu.toml` and change it:

```toml
[[social]]
    parent     = "tools"           # keep this: it puts the entry in the Tools list
    identifier = "my-tool"         # any unique name
    name       = "My Tool"         # bold text in the list
    url        = "https://u77w41.github.io/my-tool/"
    weight     = 2                 # order inside the list
    [social.params]
        description = "One short line about what it does"
        icon        = "my-tool"    # optional: assets/icons/my-tool.svg
        newTab      = true         # open in a new tab (also shows the ↗ arrow)
```

- `description` and `icon` are optional. Leave out `icon` and the entry shows just the text.
- An icon for the list works like the other icons (see **Icons** below). It is drawn at 32 px, so a simple outline logo works best.
- Any `[[social]]` entry that has children becomes a dropdown, so you could make a second one the same way, for example a "Talks" icon.

The dropdown isn't part of the theme. It is made of three files in this repo:

| File | What it does |
|---|---|
| `layouts/_partials/sidebar/left.html` | Copy of the theme's sidebar, changed to draw the dropdown (see section 5) |
| `layouts/_partials/footer/custom.html` | Opens and closes it: click, Esc, click outside |
| `assets/scss/custom.scss` | The look of the button and the list |

### Reorder or remove an item

- **Reorder:** change the `weight` numbers.
- **Remove from the menu but keep the page:** delete the `menu:` block from the page's front matter.
- **Remove completely:** delete the page folder (or the `[[main]]` block in `menu.toml`).

### Icons

`icon:` is the name of an SVG file, without `.svg`. Available now:

- In this repo (`assets/icons/`): `anurup`, `briefcase`, `brand-kaggle`, `brand-linkedin`, `brand-x`, `certificate`, `external-link`, `file-text`, `mail`, `publications`, `school`, `support`, `tools-grid`
- From the theme: `archives`, `brand-github`, `brand-twitter`, `categories`, `hash`, `home`, `link`, `messages`, `rss`, `search`, `tag`, `user`

To add a new icon:

1. Go to <https://tabler.io/icons> and search (for example "book").
2. Click the icon and choose **Copy SVG**.
3. Save it as a new file `assets/icons/book.svg`.
4. Use `icon: book`.

If an icon name has no matching file, the build stops with `icon 'book.svg' is not found under 'assets/icons' folder`. Fix the name or add the file.

---

## 5. Update the theme

The theme ([Stack](https://github.com/CaiJimmy/hugo-theme-stack)) is downloaded automatically. Its version is recorded in `go.mod`. Update it once or twice a year; there is no need to update more often.

### A. Normal update (within v4, e.g. 4.0.3 → 4.1.0)

1. See which version you have:

   ```bash
   grep hugo-theme-stack go.mod
   ```

2. Look at the [theme's releases page](https://github.com/CaiJimmy/hugo-theme-stack/releases) and read the notes for anything marked "breaking".

3. Update:

   ```bash
   hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v4
   hugo mod tidy
   ```

4. Preview with `hugo server` and click through: Projects, one project page, Resume, Certificates, the Tools dropdown, dark mode, and a narrow (phone-width) window.

   **Check the overridden sidebar.** `layouts/_partials/sidebar/left.html` is a copy of the theme's sidebar (from v4.0.3) with the Tools dropdown added, so theme updates to that file don't reach your site on their own. Compare the old and new theme versions (put the new version number in place of `4.1.0`):

   ```bash
   curl -s https://raw.githubusercontent.com/CaiJimmy/hugo-theme-stack/v4.0.3/layouts/_partials/sidebar/left.html > /tmp/sidebar-old.html
   curl -s https://raw.githubusercontent.com/CaiJimmy/hugo-theme-stack/v4.1.0/layouts/_partials/sidebar/left.html > /tmp/sidebar-new.html
   diff /tmp/sidebar-old.html /tmp/sidebar-new.html
   ```

   No output means the theme didn't touch it, and you're done. If lines are listed, copy the same changes into your `layouts/_partials/sidebar/left.html`, keeping the block marked `Added: dropdown`, and update the version in the note at the top of that file.

5. If Hugo says the theme needs a newer Hugo version, run `brew upgrade hugo`, then put the new number from `hugo version` into `HUGO_VERSION` in `.github/workflows/deploy.yml`.

6. Publish:

   ```bash
   git add go.mod go.sum .github/workflows/deploy.yml
   git commit -m "Update Stack theme"
   git push
   ```

**Changed your mind before committing?** `git checkout -- go.mod go.sum` puts the old version back.

### B. New major version (e.g. v4 → v5)

Major versions can change settings, so read the release notes first.

1. In `config/_default/module.toml`, change `/v4` to `/v5` at the end of the `path` line.
2. Download it and clean up the old one:

   ```bash
   hugo mod get github.com/CaiJimmy/hugo-theme-stack/v5
   hugo mod tidy
   ```

3. Preview carefully and fix any settings the release notes mention.
4. Commit and push as above.

### C. Update Hugo itself

The live site uses the exact Hugo version in `.github/workflows/deploy.yml`, so a new Hugo release can't break it by surprise. To move to a newer one:

1. `brew upgrade hugo`, then `hugo version` to see the new number.
2. Preview with `hugo server` and check the pages.
3. Put the number in `HUGO_VERSION` in `.github/workflows/deploy.yml`, then commit and push.

---

## 6. Change the favicon or the Support link

### Favicon (the small icon in the browser tab)

The files are in `static/`:

| File | Used by |
|---|---|
| `favicon.svg` | Modern browsers. Switches to a light stroke when the browser is in dark mode |
| `favicon.ico` | Older browsers and bookmarks (16, 32 and 48 px inside one file) |
| `apple-touch-icon.png` | iPhone/iPad "Add to Home Screen" (180 × 180 px, white background) |

To change it, replace these three files, keeping the same names. A free converter such as <https://realfavicongenerator.net> can make the `.ico` and `.png` from an SVG. Thin lines disappear at 16 px, so use a thicker stroke for the favicon than for a large logo.

Browsers cache favicons for a long time: after publishing, check in a private window.

### "Support my work" link

The page itself is in a separate repository, [U77w41/support-my-work](https://github.com/U77w41/support-my-work), and is published at <https://u77w41.github.io/support-my-work/>. This site only links to it, in two places:

- **Menu (last item):** the `[[main]]` block with `identifier = "support"` in `config/_default/menu.toml`.
- **Footer of every page:** `customText` in `config/_default/params.toml`.

Don't create `content/page/support-my-work/`: a page at the same address would clash with the separate site.

---

## 7. When something goes wrong

| Problem | Fix |
|---|---|
| Red cross in the Actions tab | Open the failed run and read the **Build** step. Run `hugo` on your Mac to see the same error, fix it and push again |
| Live site still shows the old version | Wait a minute, then press `Cmd + Shift + R` |
| `git push` asks for a password | Run `gh auth login --scopes workflow` (section 1) |
| Undo the last published change | `git revert HEAD` then `git push` |
| Need the old (2024) site | It's saved in the `old-site-2024` branch |
