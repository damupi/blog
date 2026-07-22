# blog

This project builds a new blog at `blog.damupi.com`: it grabs content damupi
has already written across other platforms (Blogger, Medium, LinkedIn,
eventually Substack) and loads it into this GitHub repo as a Jekyll site.

**Content fidelity is non-negotiable**: when importing a post from any source,
the text must be reproduced verbatim — only the format changes (source HTML/
XML → Jekyll Markdown + front matter). Never paraphrase, summarize, or
reword post content during conversion. Source images are downloaded and
hosted locally under `img/`, not hotlinked to the original platform.

Jekyll source for damupi's technical writing blog, deployed to
`blog.damupi.com` via GitHub Pages. Started as a local curation/staging
area (see Origin/Curation below) and was converted into an actual Jekyll
site in place — same repo, same posts, now buildable and deployable.

## Origin

- `blog.damupi.com` was hosted on Blogger. A Google Takeout export
  (`Takeout/Blogger/Blogs/damupi/feed.atom`) was converted post-by-post into
  Markdown files with YAML front matter (title, date, tags, category), using
  the real XML via `markdownify` — this pipeline was always faithful.
- Medium articles (profile `damupi`) are fetched from the public RSS feed
  (`medium.com/feed/@damupi`), which embeds full `<content:encoded>` HTML per
  post — reformatted to Markdown without altering wording.
- LinkedIn posts and Pulse articles (profile `dmpino`) are fetched via a
  **headed, human-in-the-loop browser session**: open a real Chrome window
  with `playwright-cli` (`--headed --persistent`, browser channel `chrome`),
  have the account owner log in manually, then scrape/scroll
  `linkedin.com/in/dmpino/recent-activity/all/` and individual
  `/pulse/...`/`/posts/...` permalinks for exact text. There is no working
  LinkedIn API/CLI approach — the `linkedin-cli` tool and its skills were
  removed after repeated auth failures; don't reinstall it.
- Substack articles (profile `damupi256603`) are fetched the same
  human-in-the-loop way, via `substack.com/@damupi256603/posts`.
- **Cross-posting**: several pieces were published on more than one platform
  (e.g. Medium + LinkedIn Pulse with identical text). When that happens,
  prefer whichever platform has the fuller version — LinkedIn posts are
  often just a short teaser linking out to the full Substack/Pulse piece.
  Don't create duplicate posts for content that's actually the same essay.

## Curation, not a raw dump

The original Blogger export had 431 posts. Only a fraction were kept.

- **Personal diary posts were deliberately excluded.** The Blogger blog was a
  15+ year personal diary (relationships, humor, health, drugs/sex jokes,
  depression, etc.) under damupi's real name. Publishing that alongside a job
  search as an analytics engineer was judged a net negative for professional
  brand/Glassdoor-style searches, so those ~408 posts were dropped rather than
  published.
- What remains in `_posts/` is the technical/professional subset: sysadmin
  (Apache, vsftpd, OTRS/LDAP), and analytics engineering (GTM, GA4, Mixpanel,
  MCP/AI agent tooling). Classification was done by actually reading each
  post's content, not by keyword matching (an early keyword-heuristic pass
  mislabeled several personal posts as professional and was thrown out).
- Borderline calls (e.g. a personal bitcoin-scam story) were resolved by
  asking the account owner directly rather than guessing.

## Structure (Jekyll site)

```
_posts/          One markdown file per post, Jekyll-standard YYYY-MM-DD-title.md
                  naming, YAML front matter (title, date, tags, category, and
                  for non-Blogger sources: source, source_url).
_layouts/         default.html (shell), post.html (article page)
_includes/        head.html, header.html, footer.html
assets/css/       style.css — hand-written, no framework
index.html        Home page: hero + paginated post-card grid
img/              Post images: Blogger posts use the Albums backup; Medium/
                  LinkedIn/Substack posts have their images downloaded
                  directly from the source (never hotlinked) and named
                  `<post-slug>-N.<ext>`. Referenced as site-absolute paths
                  (/img/...) from post bodies — Jekyll copies this directory
                  to the site root as-is.
unmatched-images.md  Post images whose source file couldn't be matched
                      locally — left pointing at the original CDN URL.
_config.yml, Gemfile, CNAME, .gitignore
```

Some old Blogger posts about Google Tag Manager contain literal
`{{Variable Name}}` syntax (GTM's own templating, not Liquid). Those posts
have `render_with_liquid: false` in front matter so Jekyll doesn't try to
parse them as Liquid tags — check for this if a GTM-related post ever shows
blank spots where `{{...}}` should render literally.

## Design

- **Palette and font pulled from `www.damupi.com`**: background `#262626`,
  card bg `#2c2c2c`/`#333`, border `#404040`, text `#eeeeee`/`#cccccc`,
  purple accent `#914bb8`, font "Anonymous Pro" monospace throughout.
- **Layout pattern inspired by a colleague's site**, andresblancog.com:
  uppercase tracked eyebrow label in the accent color, bold hero headline,
  bordered card grid with pill-shaped tags, two-button CTA style.
- Intentionally different visual identity from `www.damupi.com` itself per
  the account owner's request — same brand colors, different theme/layout.
- Header brand reads "damupi blog"; the only nav link is "www.damupi.com"
  (pointing at `site.main_site_url`). Post pages show "&larr; back to
  writing" above the title and "&larr; back to log" below the content —
  intentionally different wording, not a typo.

## Local dev

macOS system Ruby (2.6) is too old for a modern Jekyll/Liquid — a newer Ruby
was installed via `brew install ruby` (Homebrew's, kept separate from
`/usr/bin/ruby`; doesn't touch system Ruby). This is **already done** on
this machine — `bundle check` reports dependencies satisfied, no fresh
`bundle install` needed. Use it explicitly:

```
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
bundle exec jekyll serve
```

Local preview has been run and visually verified (see `.design-ref/preview_*.png`,
captured 2026-07-21). Site builds clean into `_site/`.

The `github-pages` gem (GitHub's own meta-gem pinning Jekyll 3.9) is
**not** used here — it's incompatible with modern Ruby (relies on
`String#tainted?`, removed in Ruby 3.2+). The Gemfile uses plain
`jekyll ~> 4.3` instead. Deployment therefore should go through a **GitHub
Actions** workflow (e.g. `actions/jekyll-build-pages` +
`actions/deploy-pages`), not GitHub Pages' legacy built-in Jekyll build,
so the deployed Jekyll version matches what's tested locally.

## Deployment (planned)

- Repo: `github.com/damupi/blog` (not yet created/pushed).
- Custom domain: `blog.damupi.com` is currently hosted at `blogger.com/damupi`
  (Blogger). It will be redirected to this GitHub Pages site via a
  Cloudflare DNS change, plus a `CNAME` file in this repo.
- `www.damupi.com` is a separate repo/site (`github.com/damupi/website`,
  React/Vite) and is not affected by this.

## Not done yet

- Repo not created on GitHub, nothing pushed, DNS not switched.
- No GitHub Actions workflow written yet.
