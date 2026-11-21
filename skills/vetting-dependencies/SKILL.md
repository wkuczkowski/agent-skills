---
name: vetting-dependencies
description: Vets a package before it is pulled from the network and governs bypassing the 3-day release-age cooldown in uv or pnpm. Decides whether the dependency is worth adding at all, delegates the lookup to a subagent and states a verdict. Use before uv or pip add, pnpm or npm add, npx or pnpm dlx, cargo add, go get, a Docker image, a GitHub Action, a curl-to-shell installer, a pre-built binary, or when uv/pnpm blocks a recently published package.
---

# Vetting dependencies

Applies whenever code is about to arrive from the network: a package manager add, `npx <package>` or `pnpm dlx`, `cargo add`, `go get`, a Docker image, a GitHub Action, a `curl | sh` installer, a pre-built binary. The user's explicit instructions take precedence over this skill.

## 1. Decide whether to add it at all

Code is cheap to write now. A package used for a small slice of its surface, or replaceable with a few dozen lines, is written in-project instead. State the judgement in one sentence together with the alternative, for example "one GET with no retries: `urllib` from the standard library instead of httpx". A package the user named explicitly skips this step.

## 2. Delegate the lookup

Run the check in a subagent, never in the main context; a cheaper model suffices for lookups. Give it the exact package name, the ecosystem and the version to be installed, and ask for the report in [references/checklist.md](references/checklist.md): owner and repository, last release date and release cadence, downloads or stars, open issues and maintenance signals, known CVEs or advisories, name similarity to a popular package, install scripts or postinstall hooks, licence. The subagent only reads; installation happens in the main context after the verdict.

## 3. Verdict

Weak flags the agent resolves itself and says why: an old release from a known owner, low downloads for a niche tool, a permissive licence that needs an attribution line. Three findings go back to the user before anything is installed: a typosquatting suspicion, an active CVE or advisory that affects the version to be installed, and an install script that fetches remote code. Put the finding, its evidence and the recommended choice in the question.

## 4. Release-age cooldown (3 days)

Package managers configured with a 3-day release cooldown (`uv` `exclude-newer = "3 days"`, `pnpm` `minimumReleaseAge: 4320` and `trustPolicy: no-downgrade`) refuse newly published releases by default so supply-chain compromises are caught before landing locally.

- Do not bypass the cooldown for routine adds or normal version bumps; accept the newest version older than 3 days.
- Bypass is allowed for a specific package only when there is a verified justification — such as a severe vulnerability/CVE fix or critical regression fix released within the last 3 days — and only after step 2 confirms the advisory, publisher and release integrity.
- Use the narrowest per-package override:
  - Python (`uv`): `uv add --exclude-newer-package <pkg>=false <pkg>`
  - Node (`pnpm`): pin `<pkg>@<version>` under `minimumReleaseAgeExclude` in `pnpm-workspace.yaml` (so subsequent `pnpm install --frozen-lockfile` also passes), or `--config.minimum-release-age=0` only for a one-off `pnpm dlx`.
- State the override and the verified justification in the verdict.

## 5. Install

Python: `uv add`. Node: `pnpm add` in the project, `pnpm dlx` for one-off runs.

## Done when

The dependency is installed and the verdict is stated in one or two sentences (what was checked, why it passed, and any cooldown override with its reason), or the user has been asked about a hard finding, or the sentence from step 1 says the code was written instead.
