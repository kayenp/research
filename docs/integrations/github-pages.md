# GitHub Pages

This note records the facts about the GitHub Pages site of this repository. Each fact has the label "confirmed"
or "unverified". A confirmed fact has the date of its check.

## Setup
- The repository is `kayenp/research`, and it is public.
- The site address is `https://kayenp.github.io/research/`.
- GitHub Pages serves the branch `main` from the folder `/`. The build type is `legacy` (confirmed 2026-11-19,
  `gh api repos/kayenp/research/pages`).
- Each push to `main` starts a new build. No workflow file is necessary.
- `tools/check --live` checks the published site.

## Facts
- **Plan (confirmed 2026-11-19):** the account `kayenp` has the free plan (`gh api user`).
- **Private repository (unverified):** the GitHub documents say that the free plan serves Pages only from a public
  repository. Nobody tested a private repository.
- **Build time (confirmed 2026-11-19):** the first build took less than one minute.
- **Site root (confirmed 2026-11-19):** without `index.html`, the site root gave 404. Thus `index.html` sends the
  reader to the guide.
- **Temporary 503 (confirmed 2026-11-19):** immediately after the first build, one request to the site root gave
  503. The subsequent requests gave 404. The cause is uncertain.
- **Jekyll (confirmed 2026-11-19):** without `.nojekyll`, the build made an HTML page from the Markdown report.
  The site then gave 200 for the `.md` address, for the `.html` address and for the address with no extension.
- **`.nojekyll` (unverified):** the GitHub documents say that `.nojekyll` stops the Jekyll build. Then the `.html`
  address of the report must give 404. Nobody made this check before the commit of this note.
- **Markdown type (confirmed 2026-11-19):** the site gives a `.md` file with the type `text/markdown`.

## Commit email
All readers can read the commits of a public repository. Thus the setting `user.email` of this repository is the
GitHub noreply address of the owner. This setting is local to one clone, so each new clone must set it again.
