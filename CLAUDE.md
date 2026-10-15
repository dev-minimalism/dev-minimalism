# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is the GitHub profile README for the `dev-minimalism` account. Because the repo name matches the account name, GitHub renders `README.md` at the top of https://github.com/dev-minimalism. There is no application code and no build, test, or lint step. The only deliverable is `README.md`, and the only workflow is editing it and pushing to `main`.

## How the README works

- Every badge and stat is rendered by a third-party service at view time through an image URL: shields.io (tech-stack and contact badges), github-readme-stats.vercel.app (stats card and top languages), streak-stats.demolab.com, and ghchart.rshah.org (contribution chart). Nothing is generated locally; changing what a widget shows means changing its URL query string.
- GitHub serves every README image through its camo proxy, so verify images via the rendered profile, not just the source URL. komarev.com (view counter) and github-readme-activity-graph.vercel.app were removed because camo returned 404/502 for them. Simple Icons no longer ships LinkedIn or AWS logos, so the LinkedIn badge embeds its logo as a data URI and the AWS badge is text-only. The stats cards carry `disable_animations=true` so text renders even where SVG animations do not run.
- All stats widgets use GitHub's dark theme (`theme=github_dark` on github-readme-stats, `theme=github-dark` on the activity graph). Keep new widgets consistent with this.
- Layout: a centered HTML header block, then `---`-separated sections in this order: Philosophy, Tech Stack, Focus Areas, Featured Projects, Related Organizations, GitHub Stats, Contact. Tech Stack badges are `style=flat` shields grouped under bold category labels and reflect what the linked orgs actually use (Spring Cloud, Kafka, PostgreSQL, Flutter, Terraform), so check the org repos before adding or removing one.
- Verification: there is nothing to run. Confirm the file renders as GitHub-flavored Markdown with inline HTML. A broken image in a preview is usually the external service being slow or rate-limited, not a Markdown error.

## Removed metrics workflow

A `lowlighter/metrics` GitHub Action once generated `github-metrics.svg`; the workflow was deleted in commit 49dade7 and the SVG was removed afterwards. If it is ever wanted again, recover the last workflow with:

```
git show 49dade7^:.github/workflows/metrics.yml
```

It ran on a daily cron and on push to `main`, authenticated with `secrets.METRICS_TOKEN`, and committed the SVG with a `[Skip GitHub Action]` suffix.

## Gotcha: .idea/ is tracked despite .gitignore

`.gitignore` lists `/.idea/`, but the IntelliJ files were committed before the rule existed, so Git still tracks them and IDE activity shows up as modifications (typically `.idea/vcs.xml`). Do not bundle `.idea/` changes into README commits. Untracking them would require `git rm -r --cached .idea`, which is a deliberate decision, not a side effect.

## Conventions

- Commit messages are short and frequently in Korean (e.g. `일부 텍스트 정리`); either language is fine.
- All history is direct commits to `main`. There are no feature branches or pull requests.
