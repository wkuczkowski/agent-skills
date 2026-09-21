# Interactive systems

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Mobile actions, home controls, constrained UI, games and forms.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-IN-01 — Choose the next bounded mobile-interface action](#uc-in-01) · **E**

- [UC-IN-02 — Interpret smart-home commands over a known device inventory](#uc-in-02) · **W**

- [UC-IN-03 — Compose an interface from preapproved components](#uc-in-03) · **E**

- [UC-IN-04 — Choose among legally available game moves](#uc-in-04) · **E**

- [UC-IN-05 — Select non-player-character behavior from designer-defined actions](#uc-in-05) · **P**

- [UC-IN-06 — Choose the next useful clarification in a form](#uc-in-06) · **P**


---

<a id="uc-in-01"></a>

## UC-IN-01 — Choose the next bounded mobile-interface action

**Tags:** Choice; Noul; mobile-agent; state-loop

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

The mobile-jev project demonstrates an agent that uses a device-provided interface representation to choose actions. The model is a decision component; device access and execution belong to the surrounding system.

### Concrete input example

Illustrative state: the current accessibility/UI tree, stable visible element IDs, active app, action history and a goal such as enabling dark mode. The control layer provides candidate actions and verified element bounds, not an unsupported assumption that Jev understands screenshots.

### Questions to ask

- Choice: “Which allowed next action advances this goal from the observed screen?” Options include tapping a listed control, navigation, waiting, completion and blocked.
- Speculative Choice: “Assuming the next action is a tap, which currently visible element is the target?” Include no-suitable-element.
- Noul: “Does the observed interface establish that the requested setting is now enabled?”

### Interpretation and system behavior

Code checks the UI snapshot version, resolves the chosen ID to current bounds, performs the action and observes again. It consumes only the relevant branch’s arguments. A completion claim must be verified from the resulting state, not from the model’s intention.

### Boundaries and failure modes

The repository and demos were not run here. Payment or other consequential actions require separate authorization. After an uncertain transport result, inspect actual device state instead of blindly retrying a mutation.

### How to evaluate

Test changing layouts, stale IDs, overlays, repeated taps and misleading completion screens. Measure completed goals verified by device state and the rate of unintended actions.

### Sources and related cases

[Droidrun mobile-jev](https://github.com/droidrun/mobile-jev)

Related: [UC-AG-02](agents-and-routing.md#uc-ag-02) · [UC-SW-04](software-and-security.md#uc-sw-04).


---

<a id="uc-in-02"></a>

## UC-IN-02 — Interpret smart-home commands over a known device inventory

**Tags:** Choice; Noul; fan-out; device-control

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A home-control interface can map natural language onto a limited inventory and allowed operations. The official demo illustrates fan-out and pairing with a generative component for compound requests.

### Concrete input example

`request`: “Dim the study lamp and switch off the hallway lights.” Include device IDs, room names, supported operations, current states and the user’s permissions. Never assume a device exists because its name sounds plausible.

### Questions to ask

- Noul: “Does this request contain multiple distinct device actions?”
- Choice: “For a single lighting command, which known room or device group is targeted?” Include unclear-target.
- Speculative Choice: “Assuming a lighting command, which supported action is requested?” Options describe on, off, dim and unsupported.

### Interpretation and system behavior

Code splits compound requests through a controlled mechanism, resolves exact device IDs and checks supported values. It confirms ambiguous target groups and observes the device state after execution. Open-ended conversation goes to a separate conversational handler.

### Boundaries and failure modes

Independent questions cannot see each other’s answers. Do not use this pattern as a justification for autonomous control of safety-critical appliances, locks or alarms. “All lights” must be scoped to the user’s actual authorized inventory.

### How to evaluate

Test duplicate room names, offline devices, partial compound success, stale state and unsupported brightness instructions. Measure correct affected-device sets as well as intent classification.

### Sources and related cases

[Smart-home demo](https://docs.typesafe.ai/demos/smart-home) · [Speculative fan-out](https://docs.typesafe.ai/patterns/fan-out)

Related: [UC-AG-01](agents-and-routing.md#uc-ag-01) · [UC-IN-01](interactive-systems.md#uc-in-01).


---

<a id="uc-in-03"></a>

## UC-IN-03 — Compose an interface from preapproved components

**Tags:** Choice; Noul; UI-composition; bounded-generation

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

json-render documents an experimental Jev path that assembles a UI from supplied element candidates. This is bounded composition, not arbitrary generation of HTML, code or business permissions.

### Concrete input example

Illustrative request: “Show shipment progress and a button to download the receipt.” Supply candidate components with concrete props, permitted data bindings, allowed actions and layout containers. The application already knows the user’s shipments and access rights.

### Questions to ask

- Choice per intended region: “Which available component fulfills the requested information need?” Include no-suitable-component.
- Choice: “Which allowed placement best fits the specified layout constraints?”
- Noul: “Is a required information need unsupported by the available candidates?”

### Interpretation and system behavior

Code assembles a schema-valid specification, validates component/action bindings and renders through its trusted registry. The composer does not execute button actions. Unsupported content requirements trigger an explicit fallback rather than invented props or records.

### Boundaries and failure modes

The integration was experimental and unreleased when reviewed; recheck availability and exact package contracts. This card’s questions illustrate the design, not the library’s current internal request schema. Catalog constraints do not by themselves guarantee accessible or correct UI.

### How to evaluate

Test missing components, invalid binding combinations, accessibility requirements and a requested action outside the user’s permissions. Measure task completion with the rendered interface, not merely spec validity.

### Sources and related cases

[json-render: Jev integration guide](https://json-render.dev/docs/jev) · [Vercel Labs json-render](https://github.com/vercel-labs/json-render)

Related: [UC-CC-02](commerce-and-content.md#uc-cc-02) · [UC-IN-06](interactive-systems.md#uc-in-06).


---

<a id="uc-in-04"></a>

## UC-IN-04 — Choose among legally available game moves

**Tags:** Choice; games; bounded-actions; state-loop

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

A first-hand chess experiment has used Jev to select from moves enumerated by a chess engine. This illustrates bounded decision-making, not an ability to implement the game rules through inference.

### Concrete input example

For a hypothetical turn, provide board state, side to move, relevant history and legal moves produced by the game engine. Each candidate has a stable machine ID and an understandable description. The engine owns legality and termination rules.

### Questions to ask

- Choice: “Which supplied legal move should the player choose to pursue the stated game objective?” Options are only the current legal moves.
- Optional Noul: “Does the supplied position satisfy a separately described tactical condition?” This is advisory unless verified by the engine.

### Interpretation and system behavior

Code applies the chosen move only after checking the game-state version and membership in the legal set. It updates the board and obtains a fresh move list. Compare gameplay against an appropriate engine or baseline using the same interface.

### Boundaries and failure modes

The cited experiment was not reproduced here and used an interface different from conventional text players. A legal move can be strategically poor. Do not generalize gameplay outcomes into a universal intelligence or reasoning claim.

### How to evaluate

Measure legal-action enforcement, move quality under a fixed evaluation method and results across repeated games. Include forced moves, stale positions and candidate-order sensitivity.

### Sources and related cases

[Maxim Saplin: first-hand Jev chess experiment](https://dev.to/maximsaplin/typesafe-jev-played-chess-and-landed-next-to-reasoning-models-28ga) · [Choice](https://docs.typesafe.ai/primitives/choice)

Related: [UC-IN-05](interactive-systems.md#uc-in-05) · [UC-AG-02](agents-and-routing.md#uc-ag-02).


---

<a id="uc-in-05"></a>

## UC-IN-05 — Select non-player-character behavior from designer-defined actions

**Tags:** Choice; Score; NPC; interactive-behavior

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A game designer wants flexible reactions without allowing the model to invent actions the simulation cannot execute.

### Concrete input example

`npc_state`: a merchant closing a shop, a nearby player request and the simulation’s observable world facts. Candidate actions include answer a question, direct the player to a known location, finish closing and request clarification. Dialogue text comes from templates or a separate writer.

### Questions to ask

- Choice: “Which available behavior fits the character’s current goal and observed situation?” Include no-valid-action.
- Score: “How strongly does the observed event interrupt the current non-critical activity?” Define levels using designer-provided situations rather than an unspecified urgency scale.

### Interpretation and system behavior

The game loop checks preconditions, applies cooldowns and resolves the selected behavior through ordinary simulation code. It can ignore a late response if the world state has changed. The character may then use an approved dialogue template.

### Boundaries and failure modes

This is a proposed application, not a documented real-time performance claim. Model calls should not replace physics or frame-critical control. Prevent oscillation through explicit state and hysteresis policies, and do not expose hidden game information unless intended.

### How to evaluate

Test behavioral consistency, stale decisions, contradictory goals and repeated requests. Measure player-visible responsiveness and rule violations on target hardware and service conditions.

### Sources and related cases

[Choice](https://docs.typesafe.ai/primitives/choice) · [State design](https://docs.typesafe.ai/concepts/state)

Related: [UC-IN-04](interactive-systems.md#uc-in-04) · [UC-IN-01](interactive-systems.md#uc-in-01).


---

<a id="uc-in-06"></a>

## UC-IN-06 — Choose the next useful clarification in a form

**Tags:** Choice; Noul; adaptive-forms; clarification

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A guided workflow should ask only for information still needed, using approved questions rather than generating an unpredictable interview.

### Concrete input example

`goal`: arrange repair of an office device. `known_facts`: device type and location are already supplied, but the failure is vague. `question_bank`: approved questions about observable symptoms, access times and asset identifiers, each with prerequisites.

### Questions to ask

- Choice: “Which available clarification would most directly resolve a missing prerequisite for the next workflow step?” Include no-question-needed and outside-supported-workflow.
- Noul: “Is this proposed question already answered explicitly in `known_facts`?”
- Choice: “Is the user’s latest answer responsive, partially responsive or unrelated to the previous question?”

### Interpretation and system behavior

Code renders the selected approved question, preserves prior answers and applies a maximum clarification budget. An unresolved or unsupported request can be handed to a person. Do not ask again for facts already captured by trusted tools.

### Boundaries and failure modes

The proposed design is for ordinary administrative forms, not autonomous medical or eligibility interviews. Do not infer unprovided sensitive attributes. A semantically selected next question must still pass consent, accessibility and field-validation requirements.

### How to evaluate

Test repetitive questioning, incomplete answers, changed user goals and contradictory updates. Measure successful form completion, unnecessary questions and correct escalation of unsupported cases.

### Sources and related cases

[Choice](https://docs.typesafe.ai/primitives/choice) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-IN-03](interactive-systems.md#uc-in-03) · [UC-OP-05](operations-and-support.md#uc-op-05).


[Back to all use cases](index.md) · [Handbook index](../index.md)
