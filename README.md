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

## How to

See **[HOW-TO.md](HOW-TO.md)** for step-by-step guides: preview and publish, create a new blog post or project, add a menu item, and update the theme or Hugo.

Quick version:

```bash
hugo server -D                          # preview at http://localhost:1313
hugo new content post/My-New-Project    # new project / blog post
git add -A && git commit -m "..." && git push   # publish
```

## Licences

- The Stack theme is licensed under [GPL-3.0](https://github.com/CaiJimmy/hugo-theme-stack/blob/master/LICENSE) and is pulled in as a Hugo module; it is not covered by this repository's licence.
- Everything else in this repository is under the MIT licence in `LICENSE`.
