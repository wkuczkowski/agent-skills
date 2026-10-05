---
name: grill-with-docs
description: Runs a grilling session on a plan or design and writes the project's glossary and decision records as the answers settle. Use when the user asks to be grilled with docs.
disable-model-invocation: true
metadata:
  upstream: mattpocock/skills skills/engineering/grill-with-docs
  upstream-commit: "447ca70872026d5b79d6073a546dac082117fed7"
  adopted: "2026-10-05"
---

# Grill with docs

Two skills in this collection together: `grilling` for the interview and `domain-modeling` for the documents. Read both SKILL.md files before the first round; they are in the harness's skill list and under `~/.claude/skills/<name>/` and `~/.agents/skills/<name>/`. Until 2026-10-05 this skill only said to call the Skill tool, which Codex does not have; of three Codex sessions on 2026-09-14, one loaded both skills, one only `domain-modeling`, and one neither and skipped the interview.

The documents follow `domain-modeling`: `GLOSSARY.md` and `docs/adr/` at the repo root, created when the first term or decision is settled. No other setup is needed; the `docs/agents/` files that `setup-matt-pocock-skills` writes serve issue-tracker and triage skills this collection no longer has.

Glossary entries record what the user means by his terms, taken from his answers during the grilling; writing one or an ADR is not a question for him, and each appears in the round's decided list so he can correct it. The user's instructions take precedence over this skill.
