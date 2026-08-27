# Editing this site

## Run it locally

```sh
bundle install                        # once, and after changing the Gemfile
bundle exec jekyll serve --livereload  # http://localhost:4000
```

Everything hot-reloads **except `_config.yml`** — after editing that, stop the
server with Ctrl-C and start it again.

## Publish

```sh
git add -A && git commit -m "update" && git push
```

The Actions workflow in `.github/workflows/pages.yml` builds and deploys on
every push to `main`. Watch it under the repo's **Actions** tab.

One-time setup: repo **Settings → Pages → Source: GitHub Actions**.

## Where things live

| Path | What it is |
|---|---|
| `_config.yml` | Your name, email, links, URL. Start here. |
| `_data/menu.yml` | The top nav. Edit to add/remove/reorder links. |
| `index.md` | Home page. |
| `about.md`, `projects.md`, `writing.md` | Top-level pages. |
| `_projects/*.md` | One file per project. |
| `_posts/*.md` | One file per blog post. |
| `_sass/custom.scss` | All your styling. |
| `_layouts/`, `_includes/` | Page structure. Touch rarely. |
| `assets/` | Images, your resume PDF, anything static. |

## Add a project

Create `_projects/my-thing.md`:

```markdown
---
title: My Thing
date: 2026-08-20
blurb: One line that shows in the list.
tags: [rust, systems]
repo: https://github.com/ctebow/my-thing
link:                    # optional live URL
image:                   # optional, e.g. /assets/img/my-thing.png
featured: true           # optional; used by featured_only filters
---

Markdown body here.
```

It appears automatically at `/projects/my-thing/`, in the projects list, and
(if recent) on the home page. **No other file needs editing.** Ordering is by
`date`, newest first.

## Add a blog post

Create `_posts/2026-08-20-my-title.md`. The `YYYY-MM-DD-` filename prefix is
required.

```markdown
---
title: "My Title"
date: 2026-08-20 09:00:00 -0500
blurb: Optional one-liner for the index.
tags: [notes]
---

Body.
```

## Add a standalone page

Create `contact.md` in the repo root:

```markdown
---
title: Contact
permalink: /contact/
---

Body.
```

Then add it to the nav in `_data/menu.yml`:

```yaml
  - title: contact
    url: /contact/
```

You don't need a `layout:` line — `_config.yml` defaults every page to
`layout: page`, posts to `post`, and projects to `project`.

## Images and files

Put them in `assets/img/` and reference with a leading slash:

```markdown
![Alt text](/assets/img/screenshot.png)
```

Your CV goes at `assets/resume.pdf` (the path is set by `resume_url` in
`_config.yml`).

## Restyling

Everything visual is in `_sass/custom.scss`. The variables at the top
(`$text`, `$background`, `$accent`, `$max-width`) cover most of what you'd
want to change. A dark-mode block at the bottom follows the reader's OS
setting — keep both halves in sync if you change colors.

The base theme is `no-style-please`, a ~30-line stylesheet. To see what
you're overriding:

```sh
bundle info no-style-please   # then look in _sass/ and _layouts/
```

To override a theme file, copy it into your repo at the same path — your copy
wins. `_includes/head.html`, `_layouts/default.html`, and `_layouts/post.html`
are already overridden this way.

## Liquid quick reference

```liquid
{{ site.title }}                         site-wide value from _config.yml
{{ page.title }}                         value from this file's front matter
{{ '/about/' | relative_url }}           always use this for internal links
{% for p in site.projects %}...{% endfor %}
{% include project_list.html limit=3 %}
```

Always pipe internal links through `relative_url` — it's what keeps things
working if the site ever moves to a subpath.
