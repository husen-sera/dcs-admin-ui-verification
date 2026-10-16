# DCS Admin Re-skin — Verification Report

A side-by-side check that the new DCS admin UI (django-unfold) does not break anything in the
current one, comparing **staging** against the **ui-improvement** server on identical data.

📄 **Report: https://husen-sera.github.io/dcs-admin-ui-verification/**

## Verdict

No functional regressions. Three differences were found; each is either an intended change
or an artefact of the review environment.

| Check | Result |
|---|---|
| Routes returning HTTP 200 on both | 20 / 20 |
| Admin lists with matching row counts | 17 / 17 models |
| Lists matching row-for-row by primary key | 15 / 17 |
| Filter / sort / paging / search cases | 11 / 11 return the same rows |
| Server errors or failed asset loads | 0 |
| GraphQL schema changes from the UI work | 0 — no frontend work needed |
| Migrations | 6, all metadata-only, no collision with dev |
| Emergency kill switch | exercised on the live server |

## Environments compared

| | Before | After |
|---|---|---|
| URL | staging.euclid-accounting.com | 153.127.51.205:1313 |
| Branch | `dev` @ `2a1a90fe` | `ui-improvement` @ `a7600865` |
| Admin | stock Django | django-unfold |
| Database | `dcs-app-stg` | `dcs-app-ui` (clone of stg) |

Both run on the same server and the same PostgreSQL cluster, so the same rows should come
back in the same order — and where they don't, the difference is the implementation.

## The three differences

1. **User list sorts by email instead of username** — intended; the same 36 users appear in both.
2. **Background-task history does not line up** — staging keeps writing, the clone is frozen. Data freshness, not code.
3. **ui-improvement is 8 commits behind dev** — includes a security fix. Resolved by the rebase in phase 5.

## Note

Screenshots were published with **email addresses and personal names masked**. The dots
shown in place of those values are not real data.

ClickUp: z94amcqevr / PR #754
