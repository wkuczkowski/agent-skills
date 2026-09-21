# Integration map and live implementation references

[Handbook index](index.md) · Research reviewed: 2026-09-21

This file intentionally avoids installation commands, copied SDK snippets and frozen limits. Select the route that fits the host application, then read its current contract and installed types. Do not migrate frameworks merely to add one decision call.

## Native TypeSafe routes

| Route | Read first | Follow for implementation |
|---|---|---|
| Direct HTTP | [HTTP API reference](https://docs.typesafe.ai/api) | Authentication, request and answer schemas, errors, model discovery and usage reporting. |
| Python | [Python SDK overview](https://docs.typesafe.ai/sdk/python) | [Python SDK usage](https://docs.typesafe.ai/sdk/python/usage), [Python API reference](https://docs.typesafe.ai/sdk/python/api), [Python retries](https://docs.typesafe.ai/sdk/python/api/retries), [Python exceptions](https://docs.typesafe.ai/sdk/python/api/exceptions), [Python SDK changelog](https://docs.typesafe.ai/sdk/python/changelog). |
| JavaScript / TypeScript | [JavaScript SDK overview](https://docs.typesafe.ai/sdk/javascript) | [JavaScript API reference](https://docs.typesafe.ai/sdk/javascript/api), then client, question, response, request-options and retry-policy types; [JavaScript SDK changelog](https://docs.typesafe.ai/sdk/javascript/changelog). |
| Agent-assisted development | [Official TypeSafe agent skill](https://raw.githubusercontent.com/typesafe-ai/skills/main/skills/typesafe-ai/SKILL.md) | Use this handbook for design discovery; fetch the specific live docs before coding. |

For detailed pages not fetched during this research, the [source registry](sources.md) marks them **Indexed**. Their presence in the official index is verified; their entire contents are not reproduced or certified here.

## Framework and gateway routes

| Route | Verified source and scope | Implementation caution |
|---|---|---|
| LangChain | [LangChain TypeSafe integration](https://docs.langchain.com/oss/python/integrations/providers/typesafe) and [LangChain: Building a Harness with Jev](https://www.langchain.com/blog/building-a-harness-with-jev). The integration uses a classifier/evaluation interface rather than treating Jev as an ordinary chat model. | Read current invocation and middleware types. Historical examples can use a different placement of state and questions. |
| Vercel AI SDK / AI Gateway | [Vercel: TypeSafe Jev and AI SDK](https://vercel.com/kb/guide/typesafe-jev-and-ai-sdk). The inspected guide uses the SDK’s evaluation abstraction. | Its binary question is named `boolean` and returns `probability`; the native TypeSafe primitive is Noul. Do not mix wrapper and native response fields. Recheck experimental status. |
| Netlify AI Gateway | [Netlify: TypeSafe Jev in AI Gateway](https://www.netlify.com/changelog/typesafe-jev-ai-gateway/). The announcement describes native SDK use within Netlify Functions and gateway-managed credentials. | Verify current hosting, authentication, billing and runtime requirements. Do not extrapolate its zero-configuration behavior to an arbitrary host. |
| OpenRouter | [OpenRouter TypeSafe provider listing](https://openrouter.ai/provider/typesafe). A provider listing was inspected. | A listing is not a complete integration recipe. Verify the current decision/evaluation API and account access; do not assume chat-completions compatibility. |

Native and gateway routes can differ in model identifiers, schema terminology, budgets, retries, request limits, logs and data handling. Keep one clearly defined adapter boundary. Contract-test the exact route being deployed, including uncertain answers and service failures.

## Community connectors and application frameworks

[itsmostafa/typesafe-mcp](https://github.com/itsmostafa/typesafe-mcp) is a community MCP connector exposing typed evaluation to agents. Its README describes native and gateway routes, but this research did not install it or audit its code. Treat it as an optional integration, not an official TypeSafe-managed MCP service. Inspect configuration writes, credentials, release provenance and the current tool schema before use.

[SemDecide](https://github.com/sharziki/semdecide) wraps decisions for CLI and JSONL workflows. Its exit codes and wrapper metadata belong to that project, not to the native API. Design explicit handling for errors and inconclusive decisions; never treat a failed command as a semantic negative.

[json-render: Jev integration guide](https://json-render.dev/docs/jev) describes constrained interface composition. At review time it was experimental and not yet released as the documented package API. That is a dated observation, not a permanent limitation. The live guide and repository determine current availability.

External projects are examples to inspect, not installation recommendations. Their presence here is not a security endorsement. See the [external implementation registry](sources.md#external-projects-and-integrations).

## Before the first live request

Confirm the current model identifier and supported input form, choose the official or wrapper contract, and check account access. Keep keys in the server-side or trusted local environment; do not put them in browser bundles or a committed example file. Validate questions locally and preserve raw typed results long enough to diagnose errors, under an appropriate retention policy.

Run a minimal smoke test for each primitive actually used and a mocked branch-policy test for every important outcome. Then test representative semantic cases; a successful API response only demonstrates connectivity. Set timeouts, cancellation behavior, retry budgets and cost limits that fit the application.

## Information deliberately kept live

[Current models and commercial details](https://docs.typesafe.ai/models) is the entry point for model availability, capabilities and commercial details. [HTTP API reference](https://docs.typesafe.ai/api) and the selected SDK determine current request limits and types. [TypeSafe legal documents](https://docs.typesafe.ai/legal) is the starting point for data-processing and contractual questions; a gateway’s own terms also matter. Do not copy one provider’s retention statement into another route.

For SDK upgrades, compare the installed package, changelog and current reference. The skill’s older migration link could not be fetched in this research session; rediscover a valid guide through [Live documentation index](https://docs.typesafe.ai/llms.txt) rather than guessing a replacement URL or schema.
