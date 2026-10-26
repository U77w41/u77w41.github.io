# u77w41.github.io

Personal portfolio of Ujjwal Chowdhury, live at <https://u77w41.github.io/>.

Built with [Hugo](https://gohugo.io/) and the [Stack theme](https://github.com/CaiJimmy/hugo-theme-stack) by Jimmy Cai.
Every push to `master` builds the site and publishes it to the `gh-pages` branch (see `.github/workflows/deploy.yml`).

## Where things are

| What | File |
|---|---|
| Projects (home page cards) | `content/post/<Project-Name>/index.md` + images in the same folder |
| Resume, Publications, Certificates | `content/page/<Name>/index.md` |
| Name, subtitle, avatar, footer | `config/_default/params.toml`, `assets/img/avatar.png` |
| Social icons (GitHub, LinkedIn, …) | `config/_default/menu.toml` |
| Footer wording | `i18n/en.toml` |
| Extra CSS (optional) | `assets/scss/custom.scss` |

## Common tasks

**Preview locally** (needs [Hugo extended](https://gohugo.io/installation/) and [Go](https://go.dev/dl/), e.g. `brew install hugo go`):

```bash
hugo server        # open http://localhost:1313
```

**Add a project**

```bash
hugo new content post/My-New-Project
```

Edit the new file, put a cover image next to it (named as in `image:`), set `draft: false`, then commit and push.
`weight` controls the order on the home page (1 = first). Keep `slug` unchanged once published, so links don't break.

**Update the resume link**: replace the Google Drive file in place ("Manage versions" in Drive) and the link keeps working. Otherwise edit the link at the top of `content/page/Resume/index.md`.

**Update the theme** (once or twice a year):

```bash
hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v4
hugo mod tidy
hugo server        # check that everything still looks right
```

**Update Hugo**: change `HUGO_VERSION` in `.github/workflows/deploy.yml` after testing that version locally.
It is pinned on purpose so a new Hugo release can't break the live site unexpectedly.

## Licences

- The Stack theme is licensed under [GPL-3.0](https://github.com/CaiJimmy/hugo-theme-stack/blob/master/LICENSE) and is pulled in as a Hugo module; it is not covered by this repository's licence.
- Everything else in this repository is under the MIT licence in `LICENSE`.
