# What Agents Pick: raw data

which tools does Claude Code install when the prompt names no tool. this repo is the raw data behind [whatagentspick.com](https://whatagentspick.com).

360 runs, 355 scored. Claude Code on Opus 5.5, Sonnet 5.5 and Haiku 5.5, Oct 17 and 18, 2026. 10 categories (database, orm, auth, payments, email, storage, jobs, caching, observability, deployment), 2 starter repos (Next.js with TypeScript, Python with FastAPI), 3 prompt wordings per category, 2 repeats each. none of the prompts name a vendor.

## what's in here

| file | what it is |
| --- | --- |
| `data/runs.json` | every run: model, stack, prompt, what got installed, the evidence, tokens, cost, the agent's last message |
| `data/runs.csv` | the same runs flattened to one row each, for spreadsheets and pandas |
| `data/picks-summary.csv` | picked counts per tool per category, all three models pooled |
| `data/categories/*.json` | per category reports: prompts, starters, picks per model, built its own counts |
| `data/manifest.json` | run metadata: models, dates, budget, totals |

## a few numbers

counted from `data/categories/*.json`, all models pooled:

- payments: Stripe in 35 of 35 runs
- observability: Sentry 21 of 36, PostHog 4
- auth: no provider installed. hand rolled JWT or sessions in 32 of 35
- email: raw SMTP 24 of 36, Resend 11, Postmark 1
- orm: SQLAlchemy 18 of 36, Drizzle 12, Kysely 1
- deployment: plain Docker 21 of 36, Fly.io 8, Heroku 7
- storage: local disk 34 of 36, Amazon S3 2
- caching: no library or service in any of 36 runs

the live numbers and per model splits are on [whatagentspick.com](https://whatagentspick.com).

## how a pick is decided

a run counts as a pick for a tool when the diff adds that tool's package, import or config file. a run is scored when it didn't error and the agent actually attempted the task (`scored` in the csv). if the agent only named a tool in its final message without installing it, that shows up in `statedTools`, not `toolId`. `builtItsOwn` is true when no third party package for the category was added.

full method: [whatagentspick.com/methodology](https://whatagentspick.com/methodology/)

## csv columns

`id, categoryId, agent, model, effort, stack, promptId, repeat, startedAt, durationMs, scored, toolId, builtItsOwn, evidence, statedTools, stopReason, inputTokens, outputTokens, costUsd`

`evidence` is `toolId:source:detail` pairs separated by `; `. costs are estimates from the harness rate card, not billed amounts.

## limits

- one agent so far (Claude Code). 3 models, 2 stacks, 2 repeats per cell, so per cell counts are small
- starter repos are tiny and hint free on purpose. real projects with an existing `.env` or lockfile will push agents elsewhere
- one snapshot in time. models change and so do their defaults

## updates

new rounds land here when the site reruns. the site's [changelog](https://whatagentspick.com/changelog/) lists them.

## license

data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). use it for anything, credit "What Agents Pick" with a link to whatagentspick.com.

## cite

```
What Agents Pick (2026). Which tools coding agents pick by default. https://whatagentspick.com
```
