# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is the GitHub profile README for the `dev-minimalism` account. Because the repo name matches the account name, GitHub renders `README.md` at the top of https://github.com/dev-minimalism. There is no application code and no build, test, or lint step. The only deliverable is `README.md`, and the only workflow is editing it and pushing to `main`.

## How the README works

- Every badge and stat is rendered by a third-party service at view time through an image URL: shields.io (tech-stack and contact badges), github-readme-stats.vercel.app (stats card and top languages), streak-stats.demolab.com, and ghchart.rshah.org (contribution chart); the org-wide stats card is the self-generated `github-metrics.svg` described below. Nothing is generated locally; changing what a widget shows means changing its URL query string.
- GitHub serves every README image through its camo proxy, so verify images via the rendered profile, not just the source URL. komarev.com (view counter) and github-readme-activity-graph.vercel.app were removed because camo returned 404/502 for them. Simple Icons no longer ships LinkedIn or AWS logos, so the LinkedIn badge embeds its logo as a data URI and the AWS badge is text-only. The stats cards carry `disable_animations=true` so text renders even where SVG animations do not run.
- All stats widgets use GitHub's dark theme (`theme=github_dark` on github-readme-stats, `theme=github-dark` on the activity graph). Keep new widgets consistent with this.
- Layout: a centered HTML header block, then `---`-separated sections in this order: Philosophy, Tech Stack, Focus Areas, Featured Projects, Related Organizations, GitHub Stats, Contact. Tech Stack badges are `style=flat` shields grouped under bold category labels and reflect what the linked orgs actually use (Spring Cloud, Kafka, PostgreSQL, Flutter, Terraform), so check the org repos before adding or removing one.
- Verification: there is nothing to run. Confirm the file renders as GitHub-flavored Markdown with inline HTML. A broken image in a preview is usually the external service being slow or rate-limited, not a Markdown error.

## Metrics workflow and the metrics-renders branch

`.github/workflows/metrics.yml` runs `lowlighter/metrics` (pinned to the v3.34 commit) daily and on changes to itself. It authenticates with the `METRICS_TOKEN` secret, a classic PAT with `repo` and `read:org`, so it can count private org repositories; a fine-grained PAT cannot span multiple orgs. When the PAT expires the run fails with `Bad credentials` after ~15 minutes of retries, so the fix is a new PAT via `gh secret set METRICS_TOKEN`.

The output `github-metrics.svg` is committed to the orphan branch `metrics-renders`, never to `main`, and the README loads it through its raw.githubusercontent.com URL. Constraints learned the hard way:

- metrics fetches a single page of repositories (ordered by `UPDATED_AT` desc): it stops as soon as a page returns fewer than `repositories`, and after a GraphQL timeout it halves the batch but never retries, so a too-large page silently yields zero repos and an empty languages card. A 100-repo page times out; 20 took 3.5s. Keep `repositories` equal to `repositories_batch` at 40.
- `repositories_skipped` entries are compared against the lowercased `owner/name`, so they must be lowercase.
- `plugin_languages_indepth` is off on purpose: the clone-and-analyze mode timed out in Jan 2026 and rendered only "Other".
- `plugins_errors_fatal: yes` and `debug: yes` stay on so a failed query fails the job instead of silently rendering an empty card.

## Gotcha: .idea/ is tracked despite .gitignore

`.gitignore` lists `/.idea/`, but the IntelliJ files were committed before the rule existed, so Git still tracks them and IDE activity shows up as modifications (typically `.idea/vcs.xml`). Do not bundle `.idea/` changes into README commits. Untracking them would require `git rm -r --cached .idea`, which is a deliberate decision, not a side effect.

## Conventions

- Commit messages are short and frequently in Korean (e.g. `일부 텍스트 정리`); either language is fine.
- All history is direct commits to `main`. There are no feature branches or pull requests.
