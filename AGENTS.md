# Shoesmith Life Cycle Public Site: Agent Guide

## Scope

This repository is the public source for `shoesmithlc.github.io` and the
Shoesmith Life Cycle LLC leadership site. Its deployment workflow publishes
the entire repository when changes are pushed to `RoundTable`.

Treat every change as public-facing. Do not place credentials, private notes,
customer information, internal operating materials, or unreviewed generated
content in this repository.

## Required Worktree Workflow

Use the installed `using-git-worktrees` skill before implementation work that
changes files. The baseline branch is `origin/RoundTable`.

1. Start by checking whether the current directory is already an isolated
   linked worktree. Do not create a second worktree when one already exists.
2. Fetch the latest remote state and create a short-lived feature branch from
   `origin/RoundTable`. Use a clear prefix such as `site/`, `content/`, or
   `fix/`.
3. Keep the primary checkout on `RoundTable` clean. Do not edit or commit in
   it while preparing a feature.
4. Implement and verify all work in the feature worktree.
5. Commit the focused change, push the feature branch, and request review
   before merging it into `RoundTable`.

Example:

```bash
git fetch origin
mkdir -p ../shoesmithlc-worktrees
git worktree add ../shoesmithlc-worktrees/<change-name> \
  -b site/<change-name> origin/RoundTable
cd ../shoesmithlc-worktrees/<change-name>
```

GitHub Desktop can open the created worktree as a local repository. Use its
pull, commit, and push workflow only from the feature worktree; merge to
`RoundTable` after review.

## Deployment Safety

- **`RoundTable` is the production deployment branch.** A push to it triggers
  the GitHub Pages workflow in `.github/workflows/static.yml`.
- **Never push experimental, partial, or unreviewed work to `RoundTable`.**
- **Avoid unrelated formatting churn.** Keep commits limited to one
  public-site change or correction.
- **Preserve `_config.yml` unless the task explicitly requires a site-wide
  configuration change.**
- **Keep the `assest/` directory name unchanged.** It is intentionally the
  repository's existing asset path convention; update references carefully
  rather than renaming it opportunistically.
- **Do not change the Pages workflow, custom-domain settings, or deployment
  branch without explicit owner approval.**

## Site Conventions

- **Content:** The homepage and leadership content are Markdown (`index.md`,
  `team.md`); individual executive profiles live under
  `Profile-of-team-member/`.
- **Links and paths:** Preserve GitHub Pages-compatible relative paths and
  trailing-slash conventions. Verify internal links after moving or renaming
  pages or assets.
- **Brand and governance:** Maintain Shoesmith Life Cycle LLC naming,
  executive roles, and public-facing brand voice. Flag conflicts between
  source files instead of silently picking one.
- **Accessibility:** Use meaningful headings, descriptive link text, and
  useful image `alt` text. Do not encode information in images alone.

## Verification Before Handoff

Before requesting review or merging:

```bash
git diff --check
git status --short
git diff --stat origin/RoundTable...HEAD
```

Also inspect the changed Markdown or HTML for:

1. Broken internal links and incorrect asset paths.
2. References that must use `assest/`.
3. Typos in names, roles, and external URLs.
4. Accidental private information or unrelated files.

Report the feature branch, changed files, validation performed, and the
expected public-site effect in the handoff. Do not state that the site is
published until the change has been merged into `RoundTable` and the Pages
deployment has completed.
