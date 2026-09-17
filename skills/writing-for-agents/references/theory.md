# Why the rules hold

The reasoning behind `SKILL.md`, for the case where a rule is unclear or two rules pull against each other. Every document an agent reads is run, not read: the aim is that the agent takes the same process every time, and each idea below is a lever on that variance.

Contents: [Context pointers](#context-pointers), [The two loads](#the-two-loads), [Information hierarchy](#information-hierarchy), [Completion criteria](#completion-criteria), [When to split](#when-to-split), [Leading words](#leading-words), [Negation](#negation), [Pruning](#pruning), [Invocation as a trade](#invocation-as-a-trade).

## Context pointers

A context pointer is a line that sits in the agent's context, names material that does not, and encodes the condition for reaching it. A skill's description is one. A sentence in `AGENTS.md` naming a doc is the same object. A link in a skill body to a file under `references/` is a third.

The pointer's wording usually decides whether and when the target is reached, more often than the target's quality does. A must-read file behind a vague pointer is a variance bug. Sharpen the wording first; inline the material only when sharpening fails.

A pointer does two jobs: it says what the material is, and it lists the branches that should trigger opening it. A branch is a distinct case the document handles, so that different runs take different paths. Pointers that are always loaded cost on every turn, so they are pruned harder than any body:

- Put the triggering word first. The pointer is where it works.
- One trigger per branch. Two synonyms for one branch are the same trigger written twice.
- Leave out identity the body carries anyway.

## The two loads

Everything added to a document or a pointer is paid from one of two budgets.

Context load is the cost on the agent: an `AGENTS.md` line, a skill description, anything present every turn spends tokens and attention whether or not it fires. The Claude Code skill listing and the Codex skill catalog are both capped, so this budget is finite in a literal sense.

Cognitive load is the cost on the human: knowing which documents exist and when to reach for each. The human is the index. This budget is not to be minimised; it is the price of human judgement. Spend it where judgement matters, and take it away where it does not.

Material behind a pointer escapes context load at the price of the pointer's own line. Material with no pointer at all rides entirely on cognitive load.

## Information hierarchy

A document is made of steps (the ordered actions the agent performs) and reference (definitions, rules, facts consulted on demand). Both mix freely: a recipe is all steps, a review's rule set is all reference, most skills are both. The design decision is where each piece sits on a ladder ranked by how soon the agent needs it:

1. In-file step: what the agent does, in order. The primary tier.
2. In-file reference: consulted on demand. Often a flat set of peers, which is a fine shape, not a smell.
3. Disclosed reference: a separate file reached by a pointer and loaded only when the pointer fires. Ranges from a sibling under `references/` to a file anywhere in the repo that any document can point at.

Push too little down and the top bloats; push too much and material the agent needs on every run is hidden. That tension is the whole decision.

Progressive disclosure is the move down the ladder: out of the main file, behind a pointer, so the top stays legible. Branching is the cleanest test. Inline what every branch needs; disclose what only some branches reach. When a document has steps, reference that should have been disclosed buries them, and whether the agent attends to a step becomes a coin flip.

Co-location is the within-file companion. Where the ladder decides how far down a piece sits, co-location decides what sits beside it. A concept's definition, rules and caveats stay under one heading so that reading one part brings its neighbours. The test: the document reads like documentation written for the agent. Scattering (one meaning fragmented across many places) fails it; duplication (one meaning repeated in two places) is a different fault, treated under pruning.

Sprawl is the failure mode: a document too long even when every line is live and unique. Attention thins across the excess, and every extra line is one more to keep current. The cure is the ladder: disclose reference behind pointers, and split by branch or by sequence so each path carries only what it needs.

## Completion criteria

Every step ends on a completion criterion: the condition that tells the agent the step is done. Two properties make it a lever.

Clarity: can the agent tell done from not done? A vague bound ("understanding reached") invites premature completion, where the agent ends the step early because attention has slipped to being done. The steps still visible ahead (post-completion steps) supply the pull; the criterion's clarity is the resistance. Defend in this order: sharpen the bound first, because it is local and cheap. Only when the bound is irreducibly fuzzy and the rush is observed, hide the later steps by splitting the sequence. Hiding works only across a real context boundary, such as a hand-off or a subagent dispatch; an inline call leaves the later steps in context and hides nothing.

Demand: how much the criterion requires. "Every modified model accounted for" forces thorough work where "produce a change list" does not. Demand drives legwork, the digging the agent does inside the step, latent in the wording rather than written as its own step. Demand is not step-bound: "every rule applied" binds a flat body of reference exactly as "every step done" binds a sequence, which is how an all-reference document still carries a bar for exhaustiveness.

The strongest criteria are both checkable and exhaustive. Both current models reward this: Fable 5.1 may stop before the work is finished unless done is stated, and Astra may stop to ask unless the authorised scope is stated. See `models.md`.

## When to split

Splitting one document into two spends one of the two loads, so split only when the cut earns it.

By sequence: split a run of steps when the post-completion steps tempt the agent to rush the one in front of it. Keeping them out of view drives more legwork on the current step. The reverse holds too: merging sequences exposes each step's later steps and invites premature completion.

By invocation: split off a model-invocable skill when a distinct leading word should trigger it on its own (a word actually used in prompts), or when another skill needs to reach it. The new description is permanent context load, so the independent reach has to be worth it.

## Leading words

A leading word is a compact concept the model already holds from pretraining and thinks with while running the document: lesson, fog of war, tracer bullet, tight, red. Repeated as a token, never as a sentence, it accumulates a distributed definition and anchors a whole region of behaviour in the fewest tokens by recruiting priors the model has already. A coined word works if it is defined clearly, but recruits no priors: the definition costs what a pretrained word gives free. Reach for an existing word first. A leading word replaces a repeated triad, not a single load-bearing constraint; where the substitution would trade a literal phrase for a metaphor, keep the literal phrase, since Anthropic's Fable 5.1 guidance names that trade as mannered prose.

It anchors twice. In the body it anchors execution: the agent reaches for the same behaviour each time the word appears, and inside flat reference it focuses attention on a class of thing to look for. In a pointer it anchors invocation: when the same word lives in prompts, docs and codebase, the agent links that shared language to the material and reaches it more reliably.

Hunt for refactors. A triad spelled out at three sites, a pointer spending a sentence to gesture at one idea: each collapses into a token. "Fast, deterministic, low-overhead" becomes tight. "A loop you believe in" becomes red, turning a fuzzy gate into a binary observable state. The win is double: fewer tokens, and a sharper hook for the agent's thinking. Many documents carry restatements a leading word would retire; look for them, and leave the document alone when there are none.

## Negation

Prefer the positive: state the target behaviour ("write one-line comments") so the banned one is never spoken, and put a prohibition next to its positive form so attention lands on what to do. A prohibition works on its own where the failure has a name, since a defined anti-pattern with an example is what both vendors use when a default needs correcting, and where a wrong guess costs data, security, a destructive action or a decision the user made.

## Pruning

Single source of truth: each meaning has one authoritative place, so changing a behaviour is a one-place edit. Duplication costs maintenance and tokens, and inflates a meaning's prominence on the ladder past its real rank. It is the accidental inverse of a leading word, which repeats a token on purpose and never the meaning.

The environment is a source of truth too: `package.json` scripts, config files, the directory layout, `--help` output. A document that restates it is a cache, a copy of a lookup, and a cache earns its load only when the lookup is expensive. Cache what the agent cannot find by looking: the unwritten convention, the reason behind a choice, the gotcha no config confesses. Leave one-file, one-command lookups to the environment, where they cannot go stale.

Relevance: does the line still bear on what the document does? A line loses relevance by never bearing on the task (exposition, or a branch that should be disclosed) or by going stale as the behaviour or the world changes. Shorter documents are easier to keep relevant. Without a pruning discipline the default fate is sediment: stale layers that settle because adding feels safe and removing feels risky, until someone has to core down through them to find what is still live.

No-ops: an instruction the model already obeys by default pays load to say nothing. The test is model-relative, not reader-relative: two people disagreeing about a no-op disagree about the default, and settle it by running the document, not by debate. When a sentence fails the test, delete the whole sentence rather than trim words from it. Sentences covering data, security, a destructive action or a decision the user made are reported, not deleted. The test also grades leading words: a word too weak to beat the default ("be thorough" for a model that is already thorough) is a no-op, and the fix is a stronger word, not a different technique. Both current models have raised the bar: what once needed stating is now the default, and stating it again over-triggers. See `models.md`.

## Invocation as a trade

Two invocation modes trade the two loads.

A model-invocable skill keeps its description in the harness's listing, so the agent can fire it on its own and other skills can reach it. The human can still type its name. The description is the skill's top-level pointer, forced to stay loaded: permanent context load bought for discoverability. A model-invocable skill whose content is all reference is also the home for reference several skills share, since any of them can invoke it.

A user-only skill drops its description from the listing. Only the human typing its name starts it; the model and other skills cannot. Zero context load, paid in cognitive load: the human is the index that must remember it exists. In Claude Code the human-facing text is still the description, shown in the `/` menu; in Codex it is `interface.short_description` in `agents/openai.yaml`.

Pick model invocation when the agent must reach the skill unprompted or another skill must reach it. A skill that only ever fires by hand is user-only and costs nothing in context. Sensitivity is not a reason for user-only: keep the skill discoverable and place the confirmation right before the side effect.

Reference that two user-only skills both need can live in neither, because neither can fire the other. Put it in a plain file both point at by path.

When user-only skills multiply past what the human can remember, a router skill cures the piled-up cognitive load: one user-only skill that names the others and when to reach for each. It can only hint, never fire them.

Mechanics per harness, including the third mode Claude Code offers (`user-invocable: false`, model-only), are in `harnesses.md`.
