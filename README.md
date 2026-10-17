# Task Continuity

**Finish the active task. Queue the next idea. Respect a clear change of direction.**

[![Agent Skill](https://img.shields.io/badge/Agent_Skill-Codex_%2B_Claude_Code-2563eb.svg)](skills/task-continuity/SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

You ask an agent to fix login. While it works, you remember an export feature and send another message. The agent starts building export and leaves login unfinished.

Task Continuity gives Codex and Claude Code a reusable protocol: keep one active objective, integrate corrections, queue independent additions, and finish the required verification before starting the next task.

## What happens when you send another message?

| Your message during a login fix | Agent behavior |
| --- | --- |
| "Also add CSV export." | Queue export and continue login. |
| "Use the existing auth service." | Apply the correction to login immediately. |
| "How is it going?" | Give a brief status update and resume login. |
| "An invoice app could be useful someday." | Capture an idea; do not turn it into an executable job. |
| "Pause login, do export first, then return." | Save the checkpoint and follow the explicit priority. |
| "Cancel login. Work on export instead." | Cancel login and switch. |
| "Stop." | Stop the affected work; do not automatically start queued work. |

Once login is implemented **and verified**, the next already requested task can begin. A genuine blocker keeps the current task waiting until the user supplies what is needed or authorizes a switch.

## Install

### Codex

Ask Codex:

```text
Use $skill-installer to install this skill into ~/.agents/skills:
https://github.com/akifemrebagis1/task-continuity/tree/main/skills/task-continuity
```

Invoke it with `$task-continuity`, or let Codex select it for relevant work. Automatic invocation is enabled in `agents/openai.yaml`.

### Claude Code

Clone the repository and copy `skills/task-continuity` to `~/.claude/skills/task-continuity`. Invoke it with `/task-continuity`; automatic invocation remains enabled through the standard manifest defaults.

### Keep it in effect for ongoing work

Add the [standing instruction block](integrations/global-instructions.md) to `~/.codex/AGENTS.md` or `~/.claude/CLAUDE.md`. This makes the workflow a standing expectation instead of depending only on skill selection. Preserve existing instructions. Newly installed Codex skills are available on the next turn; start a fresh host session if the installation is not detected.

The same skill content works in both hosts; only installation paths and invocation syntax differ. See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills) and [Claude Code skill documentation](https://code.claude.com/docs/en/skills).

## How it keeps its place

The agent retains an active outcome, completion criteria, next unfinished action, current constraints, and an ordered queue. Existing host task state is preferred. Long tasks can use a small private checkpoint from the included [state template](skills/task-continuity/assets/state-template.md).

Every incoming message is interpreted before the next action. Corrections update the active task; independent requests wait; explicit cancellation or priority changes take effect immediately. After an interruption or compaction, the agent checks existing results and resumes the unfinished step.

Task state belongs to one conversation. It must not leak into the public skill repository or become another chat's queue. Reaching the front of the queue does not grant missing permissions for publication, deletion, purchases, or messages.

## Evaluate the behavior

The [16 evaluation cases](skills/task-continuity/references/behavior-cases.md) cover independent additions, mixed messages, corrections, questions, ideas, explicit switching, stopping, interrupted tools, blockers, permissions, compaction, and premature completion.

One [live Codex conversation passed on 17 October 2026](docs/codex-live-validation-2026-10-17.md): an independent second request arrived mid-turn, task A was completed and independently validated, and task B started afterward. The run used installed Codex 0.158.0 with `gpt-6-sol`; all 10 recorded assertions passed.

A second [stop-with-tool-in-flight test](docs/codex-stop-validation-2026-10-17.md) had a **partial-pass result**. An acknowledged queued task never started after a natural-language stop, and the running job was canceled before its pending write. However, cancellation took about 35.5 seconds and the host started three status-only continuation turns for a persistent goal. The original harness's stronger zero-following-turns assertion failed (11/12 assertions passed). This is evidence for queued-work suppression in that run, with clear limits on immediate cancellation and host scheduling.

The 16 cases remain evaluation specifications, not a completed benchmark. These two live scenarios used Codex app-server; desktop UI input handling, native client Queue removal, and Claude Code were outside their scope. This is an instruction-only skill: it does not modify the app's message scheduler, prevent host interruptions, or guarantee model compliance. It helps the agent preserve the correct objective when execution resumes.

## Contribute

Useful contributions include anonymized transcripts showing an actual task switch, a minimal reproduction, the expected next action, and host/model/version details. Keep the core protocol concise and preserve the user's ability to stop or change priorities. Do not add private conversations, credentials, or user files.

If Task Continuity solves a recurring interruption problem for you, a star helps others find it.

MIT licensed. No scripts, background processes, telemetry, or external service dependencies.
