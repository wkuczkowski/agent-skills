---
name: domain-modeling
description: Records what the user means by his project terms in GLOSSARY.md, and hard-to-reverse decisions as ADRs. Use when the user explains or corrects what he means by a word, uses a project term differently from the glossary or code, or when writing or editing a GLOSSARY.md or an ADR.
metadata:
  upstream: mattpocock/skills skills/engineering/domain-modeling
  upstream-commit: "d80fa0f4ebe0c5714af0adf8670336065233ecc6"
  adopted: "2026-10-05"
---

# Domain modeling

The glossary is worth having because it records what the user means by his words: "harness", "own skill", "bieg", "notatka". An agent can find what the code calls a thing on its own; what he means when he says it is the part a new session cannot guess, and the part that has cost rework when guessed wrong. Reading `GLOSSARY.md` for vocabulary is something any session does; this skill is for changing it.

## Where his meaning comes from

- His own messages are the main source. When he uses a project word with a meaning the glossary lacks, or differently from it, that is a glossary change, written in his sense, with his wording kept where it carries the meaning.
- Code, documents and legal texts settle what the project's things are and do. A term from a statute or regulation is defined by its source, cited.
- Where his usage and the code or glossary disagree, he decides which wins; that is the one kind of glossary question that goes to him, in one sentence with the two meanings side by side and a recommendation. A difference the agent can resolve from his earlier messages or the docs is not a question.
- He dictates by voice, so a strange word may be a mishearing of a glossary term rather than a new one.

## Writing

- Terms are written as soon as they settle, not batched, and each change is mentioned in a line of the reply so he can correct it. The entry uses his term as the headword, with an English gloss when the term is Polish.
- The glossary holds meanings only, never implementation details, specs or plans. Format: [references/glossary-format.md](references/glossary-format.md).
- An ADR records a decision that is hard to reverse, surprising to a later reader without context, and the result of a real trade-off; without all three it is not worth one. Format: [references/adr-format.md](references/adr-format.md).
- `GLOSSARY.md` sits at the repo root and ADRs in `docs/adr/`, both created when the first entry exists; a `GLOSSARY-MAP.md` at the root means one glossary per context.

The user's instructions take precedence over this skill.
