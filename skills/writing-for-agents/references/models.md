# Model notes

How the two current models react to instruction text, and what that changes in the writing. Sources: `research/claude-code-skills-and-fable-2026-09-06.md` (section 3, Claude Fable 5.1, released 2026-09-01) and `research/codex-skills-and-astra-2026-09-06.md` (section 3, GPT-6 Astra, released 2026-09-03, bundled default in codex-cli 0.153.4). Both models follow instructions more closely than their predecessors, so every rule here is a rule about saying less, stated once.

## Claude Fable 5.1

Instruction following (`research/claude-code-skills-and-fable-2026-09-06.md`, sections 3.4 and 3.5). Most behaviours are steered by a brief instruction; enumerating each behaviour by name is unnecessary and reads as over-prompting. Emphasis of the "CRITICAL: You MUST" kind, and hedges like "if in doubt, use the tool", make the model over-trigger; write "Use this tool when ...". Anthropic's own note: skills developed for prior models are often too prescriptive for Fable 5 and can degrade output; remove older instructions where the default is better.

Reasoning (section 2.1). An instruction to show, echo or reproduce its internal reasoning as response text can trigger a `reasoning_extraction` refusal. Ask for the result and its evidence.

Finishing (section 3.3). Fable 5.1 batches fewer implied tool calls than Fable 5 in loops where the next step is implied rather than requested, and sometimes ends a turn by describing what it would do next, or stops to ask permission for a step the request already covered. A skill therefore states the completion criterion and the authorised scope. Anthropic's supplied form: the user is not watching in real time; before ending the turn, check whether the last paragraph is a plan, a question or a promise, and if so do that work now.

Scope (sections 3.3 and 3.4). It may fix nearby code, extend behaviour the task did not mention, or commit more test files than the change warrants, and it responds well to explicit instructions about what to leave out. It is more likely than Fable 5 to rewrite a whole file; say "edit surgically when it does not change the result". When the user describes a problem or thinks aloud, the deliverable is an assessment, and the model should report and stop.

Output (section 3.3 and 3.5). It writes fewer progress updates during long tool-calling turns and uses less chat formatting than earlier models. Replace anti-formatting language in older documents with a rule that says when specific formatting is appropriate, or drop it; where the same document also serves Astra the rule stays, since Astra's default runs list-heavy; if updates are wanted, ask for a one-line opener and a short standalone recap. At low effort it searches less and answers from memory more; raise the effort or add a verification nudge for lookups.

Claude Code specifics (section 2.1). Write concise skill bodies that state what to do rather than narrating how or why. Put the key use case first in the description because the listing truncates. Use `disable-model-invocation: true` for side-effecting workflows meant to be started by hand.

## GPT-6 Astra

Instruction sensitivity (`research/codex-skills-and-astra-2026-09-06.md`, section 3.3). Astra is stronger at general instruction following and more sensitive to instructions in skills and files such as `AGENTS.md`; OpenAI recommends auditing those files. Unclear or conflicting guidance in a skill can make it pause and block work early. Make the priority explicit in the skill: the user's instructions take precedence over the skill's guidelines. OpenAI also asks the model to name and quote the `SKILL.md` line responsible whenever a skill makes it pause or leave work unfinished, so every constraint in a skill should survive being quoted back.

Initiative (section 3.3). It asks a clarifying question more often when input could change the result, and may stop where a user expected it to assume and persist. State the autonomy boundary: complete what is already authorised and reversible, ask only before destructive or irreversible steps, treat "can you ..." as an instruction to do the work.

Output (section 3.3). It defaults to detailed, formatted, list-heavy responses and may repeat phrases across sessions. Where prose is wanted, ask for it: clear paragraphs that each develop one idea, lists only for material that is parallel or sequential, the main point early. Its own guide lists filler to avoid (concluding summaries, "it's worth noting", contrastive "X, not Y" framings).

Delegation and testing (section 3.3). It delegates to subagents less often than desired; say when and how much to parallelise. It tests thoroughly before calling a task complete, which on a small change means broader tests than needed; say which checks suffice and that broader testing follows only from new changes, failures or open concerns.

Codex skill authoring (section 2.2, the bundled `skill-creator`). Assume Codex is already capable and include only what changes its decisions. Match specificity to the risk: absolute language only where correctness, safety, permissions or a fragile workflow require it. Keep discovery cheap: describe the capability and when it applies, with exclusions only where they prevent likely misrouting. Keep automatic selection on unless the user asks for explicit-only, and never infer explicit-only from sensitivity.

## Both

- The no-op test (would removing the line cause a mistake in a session?) has a higher bar on both models: instructions that were needed against laziness or under-triggering on older models now over-trigger. Where a line's necessity is in doubt, run the document without it before keeping it. Lines covering data, security, a destructive action or a decision the user made are kept without the test.
- Both reward an explicit completion criterion and an explicit scope, for opposite reasons: Fable 5.1 may stop early or drift wide, Astra may stop to ask or test too much.
- Neither needs to be told to be thorough, to think step by step, or to verify in general; the exception is Fable 5.1 at low effort, where the verification nudge for lookups above still helps. A stronger leading word replaces a weak one; a default behaviour is left unsaid.
