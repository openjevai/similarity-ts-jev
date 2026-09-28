# OpenJEV support

This fork adds optional [OpenJEV](https://openjev.sh) support alongside the
original TypeSafe integration. TypeSafe remains the default; OpenJEV is a
community gateway to the same Jev model.

## What was added

- **`src/cli.ts`** — `resolveProvider()` selects the Jev provider:
  1. `JEV_PROVIDER=openjev` (explicit) → OpenJEV.
  2. `JEV_PROVIDER=typesafe` (explicit) → TypeSafe.
  3. No explicit choice + `TYPESAFE_API_KEY` set → TypeSafe (unchanged default).
  4. No explicit choice + only `OPENJEV_API_KEY` set → OpenJEV.
  `createClient()` now passes `apiKey` to the `@typesafe-ai/sdk` `TypeSafeClient`
  constructor when OpenJEV is selected, so the key comes from `OPENJEV_API_KEY`
  instead of `TYPESAFE_API_KEY`. OpenJEV defaults: endpoint
  `https://api.openjev.sh`, model `openjev` (overridable via `--base-url`,
  `--model`, or `OPENJEV_DEFAULT_MODEL`). The `--model` and `--base-url` option
  descriptions and the error message now mention OpenJEV.
- **`README.md`** — OpenJEV note after the intro, Environment section docs, and
  option table updated with OpenJEV defaults.

No TypeSafe code was renamed, removed, or re-defaulted. The `@typesafe-ai/sdk`
dependency, `TYPESAFE_API_KEY`, `TYPESAFE_BASE_URL`, and `TYPESAFE_DEFAULT_MODEL`
all work exactly as before.

## How to configure

```bash
# Auto-detect: only OPENJEV_API_KEY set → OpenJEV
export OPENJEV_API_KEY=...
npx @kongyo2/similarity-ts-jev .

# Or force OpenJEV explicitly
export TYPESAFE_API_KEY=...   # still set for other tools
export OPENJEV_API_KEY=...
JEV_PROVIDER=openjev npx @kongyo2/similarity-ts-jev .
```

## How it was verified

- A live `POST https://api.openjev.sh/v1/systemone` request with model `openjev`,
  state `ping`, and one `noul` question returned HTTP 200.
- `grep` confirmed no hardcoded `api.typesafe.ai` default was introduced; the
  only `api.typesafe.ai` references are the original TypeSafe defaults (SDK and
  README), which are unchanged.

## Upstream

Original project: https://github.com/kongyo2/similarity-ts-jev by @kongyo2
