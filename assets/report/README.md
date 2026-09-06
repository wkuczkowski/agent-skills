# Report template

House style for every HTML report the user reads: Folk palette, paper texture, straight corners, numbered sections with a sticky contents column, in-page search, offline (nothing loads from the network). `template.html` holds the CSS and JavaScript; `build` fills it. A report is one self-contained file under `reports/<YYYY-MM-DD>/`.

## Build

1. Write the sections as an HTML fragment (`sections.html`, kept next to the output or in a temporary directory): one `<section id="...">` per report section, each starting with `<h2>`. Section ids are lowercase letters, digits and hyphens and become the anchors.
2. Run `assets/report/build sections.html --out reports/<date>/<name>.html --title "..." --subtitle "..." --eyebrow "..." --chip "Key=Value" ... --foot "..."`. `--help` lists the options.
3. Read the output line. The script numbers the sections, builds the contents, expands `data-src`, colours diffs and then checks: unique ids, every `href="#..."` resolves, no network resources, no unfilled placeholder. Exit 1 with `problem:` lines means fix the fragment and build again; the file is still written so the problem can be seen.

The build has run clean when it exits 0 and every number, path and quotation in the fragment came from this run's data.

## Components

Plain HTML plus these classes. Escape `<`, `>` and `&` in any text taken from files or transcripts, or let `data-src` do it.

| Purpose | Markup |
|---|---|
| Opening ledger | `<div class="brief"><div class="brief__row"><div class="brief__k">Key</div><div class="brief__v">Value</div></div>...</div>` |
| Lead paragraph, body text | `<p class="lead">`, `<p class="prose">`, `<div class="prose">` (measure-limited) |
| Table | `<div class="table-wrap"><table class="tbl"><thead><tr><th>...</th></tr></thead><tbody><tr><td data-l="Column">...</td></tr></tbody></table></div>`; `data-l` labels the cell on phones; `class="num"` right for numbers; `tr.is-warn`, `tr.is-alarm`, `tr.is-na` colour a row; `tbl--num` on the table when the first column is a row number |
| Status pill | `<span class="pill pill--ok">recommended</span>`; variants `ok`, `warn`, `note`, `no`, `alarm`, `na` |
| Callout | `<div class="callout callout--note"><div class="callout__lb">Label</div><p>...</p></div>`; variants as pills |
| Proposal card | `<div class="card"><div class="card__head"><span class="card__id">P-01</span><h3>Title</h3><span class="pill pill--ok">strong</span></div><div class="card__body">...</div></div>` |
| Evidence line | `<p class="ev" id="E-01"><span class="loc">E-01 · claude-code · <a href="file:///home/user/.claude/projects/x/session.jsonl">/home/user/.claude/projects/x/session.jsonl</a>:123 · 2026-09-02 14:05</span><br><q>short quoted excerpt</q></p>` |
| Raw file | `<pre class="code" data-src="usage.txt"></pre>` (path relative to the fragment, or absolute) |
| Diff | `<pre class="diff" data-src="upstream-x.diff"></pre>` or `<pre class="diff">` with escaped inline text; lines starting with `+`, `-`, `@@`, `---`, `+++` are coloured |
| Long block folded | `<details><summary>SKILL.md draft</summary><pre class="code">...</pre></details>`; search opens a folded block that holds a match |
| Numbered steps | `<ol class="steps-ol"><li>...</li></ol>` |
| Definitions | `<dl class="defs"><div><dt>term</dt><dd>meaning</dd></div></dl>` |
| Checklist | `<ul class="checklist"><li><span class="y">✓</span><span>done</span></li></ul>` |
| Missing fact | `<span class="ph">unknown: reason</span>` where a value could not be established; never a guessed value |

Links to local files use `file://` with an absolute path. The pills and callouts carry their meaning in the label text as well as the colour: a proposal is `proposed`, never styled as if applied.

## Sample

```html
<section id="summary"><h2>Summary</h2>
<div class="brief">
  <div class="brief__row"><div class="brief__k">Window</div><div class="brief__v">2026-08-24 to 2026-09-06</div></div>
  <div class="brief__row"><div class="brief__k">Proposals</div><div class="brief__v"><span class="mono">3</span> strong, <span class="mono">2</span> medium</div></div>
</div>
</section>
```
