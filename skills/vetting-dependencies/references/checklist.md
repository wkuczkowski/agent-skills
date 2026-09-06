# Lookup checklist

What the subagent reports, and where each fact is looked up per ecosystem. Use whichever of web fetch, `curl`, `gh` and the registry CLIs is available; name the source next to each fact.

## Report

One block per package, facts before judgement:

1. Owner and repository: registry owner, linked source repository, whether the two match.
2. Last release: date and version; release cadence over the last two years; archived or deprecated flags.
3. Reach: downloads per month (registry) or stars (repository).
4. Maintenance: open issues and pull requests, date of the last commit, number of maintainers with publish rights.
5. Advisories: known CVEs or advisories for the version to be installed, and whether they are fixed in a later version.
6. Typosquatting: the most popular package with a similar name, when the candidate is not that package. One changed or dropped character, a swapped hyphen or underscore, added digits, a scoped versus unscoped name, or a package created shortly after a popular one are all signals; compare creation dates and download counts.
7. Install-time code: install, preinstall or postinstall scripts, `build.rs`, a source distribution that runs `setup.py`, an installer script that fetches further remote code; quote the URL it fetches from.
8. Licence: SPDX identifier and whether it needs an attribution line or has copyleft terms.
9. Recommendation: install as is, install a different version, ask the user (naming which of the three hard findings applies), or write the code in-project.

## Sources per ecosystem

**PyPI.** `https://pypi.org/pypi/<name>/json`: `info.project_urls`, `info.license`, `releases` with upload times, `info.yanked`. Downloads: `https://pypistats.org/api/packages/<name>/recent`. Wheels run no code at install; a source distribution runs `setup.py`, so check whether a wheel exists for the platform.

**npm.** `pnpm view <name> --json` or `npm view <name> --json`: `time`, `maintainers`, `repository`, `license`, `scripts` (`preinstall`, `install`, `postinstall`), `deprecated`. Downloads: `https://api.npmjs.org/downloads/point/last-month/<name>`. pnpm 10 skips dependency lifecycle scripts unless they are allowlisted in `pnpm.onlyBuiltDependencies`; a package that needs its postinstall to work says so in its README.

**crates.io.** `https://crates.io/api/v1/crates/<name>`: `crate.downloads`, `crate.recent_downloads`, `crate.repository`, `versions[].created_at`, `versions[].license`. `build.rs` and proc macros run at build time. Advisories: RustSec, `https://rustsec.org/packages/<name>.html`.

**Go.** `https://pkg.go.dev/<module>` for the repository, licence and imported-by count; `https://proxy.golang.org/<module>/@v/list` for versions. Advisories: `https://vuln.go.dev` or `govulncheck` after adding.

**Docker images.** Docker Hub: `https://hub.docker.com/v2/repositories/<namespace>/<name>/` (`pull_count`, `last_updated`, `is_official` under `library/`) and `.../tags?page_size=10`. GHCR and other registries: the linked source repository and its release page. Note whether the tag is immutable (a digest) or floating (`latest`, a major version).

**GitHub Actions.** The action's repository (below), the Marketplace page for the verified-creator badge, and `action.yml`: `runs.using` (`node20`, `composite`, `docker`) and any step that downloads a script or binary from outside the repository. Advisories: `gh api "/advisories?ecosystem=actions&affects=<owner>/<repo>"`.

**Installer scripts and pre-built binaries.** Download the script to a file and read it before anything runs: which URLs it fetches, whether it verifies a checksum or signature, where it writes, whether it needs root. For a binary: the release page, its checksum file and signature, and whether a package manager (apt, `uv tool install`, `pnpm dlx`, `cargo install`) ships the same tool.

**Any GitHub repository.** `gh api repos/<owner>/<repo>`: `stargazers_count`, `pushed_at`, `open_issues_count`, `archived`, `license.spdx_id`. `gh api repos/<owner>/<repo>/releases?per_page=5` for release dates. `gh api repos/<owner>/<repo>/security-advisories` for published advisories.

**Advisories across ecosystems.** `https://api.osv.dev/v1/query` with `{"package": {"name": "<name>", "ecosystem": "<PyPI|npm|crates.io|Go>"}, "version": "<version>"}`; an empty `vulns` list is a clean result for that version.
