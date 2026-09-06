# Transcript sources

These layouts and fields were verified on this machine on 2026-09-05 and re-checked by the first full run on 2026-09-06. Inspect current records before relying on them. Prefer streaming JSONL with a JSON parser; raw text search can identify candidates but cannot establish authorship. Existing Python standard-library tooling is sufficient.

Resolve the actual home/config root rather than hardcoding a username. Codex normally uses `~/.codex`, with `CODEX_HOME` as an override. Claude Code normally uses `~/.claude`; check `CLAUDE_CONFIG_DIR` when configured. Inspect only relevant configuration values, not credential files.

## Codex

Primary transcripts:

- `sessions/YYYY/MM/DD/rollout-*.jsonl`
- `archived_sessions/**/*.jsonl`, searched recursively

Read `session_meta.payload` first. Its `id` identifies the transcript; a subagent's `session_id` can equal its root's ID. Check `thread_source` and `source` before classifying message authorship. Sources observed on this machine (2026-09-06): `cli` and `vscode` are human surfaces; `exec` is a headless run launched by an agent; `subagent` is agent-authored; `guardian_review`, `voice_chat` and `agent_created_thread` also occur upstream. Automatic-review prompts are agent-authored. In a voice (realtime) record only the `<input>` element carries the user's words; the surrounding handoff text is system text. Agent-created tasks need their actual human turns established rather than accepting all their content.

Select `type=response_item`, `payload.type=message`, `payload.role=user`. For recent records, inspect `payload.internal_chat_message_metadata_passthrough.content_item_kinds`, aligned by index with `payload.content`:

| Content kind | Use |
| --- | --- |
| `user.text` with text content | Candidate human evidence after session-origin checks |
| `user.image` | Attachment context, not an independently authored preference |
| `agents_md.instructions` | Injected guidance, not new feedback |
| `environments.environment_context` | Runtime context |
| `plugins.recommendations` | Generated recommendations |
| `skills.selected_skill_instructions` | Loaded skill instructions |

Preserve message ID, metadata `turn_id`, metadata `create_time`, outer `timestamp` and `ordinal`, plus an internal file/line locator. `create_time` uses epoch seconds. Outer timestamps are ISO UTC; filenames can use local time. Image wrappers and quoted/attached documents within actual user text still need separating from the user's own request.

Unknown or missing kind metadata requires a compatibility check, not unconditional acceptance of `role=user`. Legacy `event_msg` / `payload.type=user_message` records can supply a fallback, but overlap response messages. Deduplicate overlapping event and response representations of each user message; preserve distinct messages within the same turn.

Subagent linkage uses `parent_thread_id` and structured `source.subagent.thread_spawn`. Forks can have `forked_from_id`; some records have `subagent_history_start_ordinal`. Deduplicate inherited messages by original message ID and lineage. When IDs differ, compare timestamp and normalized text in the known lineage before counting repetition. Do not merge independent feedback merely because the wording or start time matches.

### Indexes and fallback sources

Current local SQLite names include `state_5.sqlite` and `thread_history_1.sqlite`; version suffixes may change. Inspect table schemas first.

- `threads` indexes IDs, `rollout_path`, source, archive state and creation/update timestamps.
- `thread_spawn_edges` links `parent_thread_id` and `child_thread_id`.
- `thread_items` contains `thread_id`, `turn_id`, `item_id`, `rollout_ordinal`, `created_at_ms`, `item_type` and `item_json`. `userMessage` items are a projection, not independent evidence.
- `thread_turns` can link turns to rollout offsets and ordinals.
- `history.jsonl` has `session_id`, `ts` and `text`; `session_index.jsonl` has IDs, names and update times. Neither supplies complete provenance or coverage.

Use indexes to locate original records or corroborate missing metadata. Do not count their copies again.

## Claude Code

Primary transcripts:

- `projects/<encoded-project-path>/<session-uuid>.jsonl`
- Subagents at `projects/<encoded-project-path>/<parent-session-uuid>/subagents/agent-<agent-id>.jsonl`

The directory encodes a project path, but read `cwd` for project identity rather than trying to reverse a lossy slug. User records contain `type=user`, `message.role=user`, `message.content`, `uuid`, `parentUuid`, `sessionId`, `timestamp`, and provenance fields.

`message.content` may be a string or a block list. Extract only text blocks at the message level; text inside `tool_result` remains a tool result.

Apply these authorship checks:

1. Exclude subagent paths and records with `isSidechain=true` or `agentId` from human evidence.
2. Exclude `isMeta`, `isCompactSummary`, non-user records, tool results and `promptSource=system`.
3. Exclude `origin.kind=task-notification` and generated wrappers such as `task-notification`, `local-command-stdout`, `bash-stdout` and `bash-stderr`. They can occur without an `isMeta` flag. In a mixed message, classify segments separately.
4. `origin.kind=human` with `promptSource=typed` or `queued` is strong authorship evidence. `suggestion_accepted` records a selected suggestion; interpret what the human accepted within that scope.
5. `promptSource=sdk` or `entrypoint=sdk-cli` alone is ambiguous. It can represent another agent's headless prompt, but verified desktop SDK records also had `origin.kind=human`. Require corroborated human origin rather than accepting or rejecting every SDK record.
6. Legacy records without provenance may be corroborated against `history.jsonl` using session ID and matching input. That file contains `display`, `pastedContents`, `timestamp`, `project`, `sessionId`; timestamps are Unix milliseconds. Missing matches do not prove automation. Pasted and queued inputs can differ. Keep unresolved authorship uncertain.

`parentUuid` is message ancestry, not the parent session ID. For subagents, use the enclosing directory, `sessionId` and `agentId`. Preserve UUID, session ID and ISO UTC timestamp, plus an internal file/line locator. Deduplicate UUIDs across copied/resumed history, then use lineage and timestamp/text comparison when needed. Compaction summaries are generated restatements, not fresh user evidence.

## Reading live history

Stream transcripts without editing them. An incomplete final JSONL line can be an active write; skip and report it, or reread later. Report malformed records elsewhere as coverage gaps.

For SQLite exploration, use read-only access. For sustained analysis, prefer a coherent temporary snapshot using SQLite's backup API from a read-only connection. A plain copy of a live `.sqlite` file can omit WAL data; if copying files, account for WAL/SHM and concurrent writes. Keep snapshots and any extracted private text in a private temporary directory, outside tracked skill assets.

When a source is unavailable, check its configured root, archives, indexes and history before reporting the gap. Do not broaden the date range or project scope silently. In the report, distinguish unavailable history, excluded automated content, uncertain authorship, and examined human feedback.
