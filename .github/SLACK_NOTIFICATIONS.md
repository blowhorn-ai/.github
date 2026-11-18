# Slack notifications for this repo

## What runs

`.github/workflows/slack.yml` ("Slack Notify") posts to Slack when:

- Someone stars this repo — `star-notify` job, `watch` event, type `started`.
  Message: the starring user, the repo, and the current star count.
- An issue is labeled `good first issue` or `first-timers-only` —
  `good-first-issue-notify` job, `issues` event, type `labeled`.
- An issue is labeled `help wanted` — `help-wanted-notify` job, same trigger.

It uses `slackapi/slack-github-action@v2.1.1` with `chat.postMessage`, the same
pattern as `blowhorn-ai/blowhorn-site`. The token is referenced only as
`secrets.SLACK_BOT_TOKEN`; no secret value is committed anywhere in this repo.

## What the workflow needs to actually deliver

The workflow file alone is not enough: `SLACK_BOT_TOKEN` must resolve in this
repo. That means an org secret named `SLACK_BOT_TOKEN` granted to
`blowhorn-ai/.github`, or a repository secret of the same name. Granting the
secret is a settings action, not a code change — it needs someone with org
admin (or repo admin) access.

## Current state (verified 2026-11-07)

- The workflow is on `master` (commit `6811146`, Oct 26) and matches the
  blowhorn-site pattern, so no workflow change is needed for the star
  notification itself.
- It is not delivering yet: the latest star-triggered run (Nov 6) fails at the
  `Notify Slack` step, and this repo has no secrets configured, so the token
  does not resolve.
- Once the secret is granted, the next star — or bouncing the
  `good first issue` / `first-timers-only` / `help wanted` label on any issue — should produce a
  Slack message; a green run on that event confirms the fix end to end.
