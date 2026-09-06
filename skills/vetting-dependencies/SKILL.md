---
name: vetting-dependencies
description: Vets a package before it is pulled from the network. Decides whether the dependency is worth adding at all, delegates the lookup to a subagent and states a verdict. Use before uv or pip add, pnpm or npm add, npx or pnpm dlx, cargo add, go get, a Docker image, a GitHub Action, a curl-to-shell installer, or a pre-built binary.
---

# Vetting dependencies

Applies whenever code is about to arrive from the network: a package manager add, `npx <package>` or `pnpm dlx`, `cargo add`, `go get`, a Docker image, a GitHub Action, a `curl | sh` installer, a pre-built binary. The user's explicit instructions take precedence over this skill.

## 1. Decide whether to add it at all

Code is cheap to write now. A package used for a small slice of its surface, or replaceable with a few dozen lines, is written in-project instead. State the judgement in one sentence together with the alternative, for example "one GET with no retries: `urllib` from the standard library instead of httpx". A package the user named explicitly skips this step.

## 2. Delegate the lookup

Run the check in a subagent, never in the main context; a cheaper model suffices for lookups. Give it the exact package name, the ecosystem and the version to be installed, and ask for the report in [references/checklist.md](references/checklist.md): owner and repository, last release date and release cadence, downloads or stars, open issues and maintenance signals, known CVEs or advisories, name similarity to a popular package, install scripts or postinstall hooks, licence. The subagent only reads; installation happens in the main context after the verdict.

## 3. Verdict

Weak flags the agent resolves itself and says why: an old release from a known owner, low downloads for a niche tool, a permissive licence that needs an attribution line. Three findings go back to the user before anything is installed: a typosquatting suspicion, an active CVE or advisory that affects the version to be installed, and an install script that fetches remote code. Put the finding, its evidence and the recommended choice in the question.

## 4. Install

Python: `uv add`. Node: `pnpm add` in the project, `pnpm dlx` for one-off runs.

## Done when

The dependency is installed and the verdict is stated in one or two sentences (what was checked, why it passed), or the user has been asked about a hard finding, or the sentence from step 1 says the code was written instead.
