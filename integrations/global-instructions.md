# Make task continuity a standing instruction

Skill discovery is selective. To apply the workflow to ongoing work by default, append the relevant block to the host's existing global instruction file. Preserve its existing contents.

## Codex: `~/.codex/AGENTS.md`

```markdown
## Task continuity

For multi-step work and incoming messages during an unfinished task, read and apply
`~/.agents/skills/task-continuity/SKILL.md`.
Keep one active objective and its completion criteria. Integrate related corrections,
answer brief status questions, and queue independent additions in arrival order.
Resume the next unfinished action and verify the active outcome before promoting
queued work. Honor explicit stop, pause, cancellation, replacement, and priority
changes immediately. Preserve these rules across compaction without reviving canceled work.
```

## Claude Code: `~/.claude/CLAUDE.md`

```markdown
## Task continuity

For multi-step work and incoming messages during an unfinished task, read and apply
`~/.claude/skills/task-continuity/SKILL.md`.
Keep one active objective and its completion criteria. Integrate related corrections,
answer brief status questions, and queue independent additions in arrival order.
Resume the next unfinished action and verify the active outcome before promoting
queued work. Honor explicit stop, pause, cancellation, replacement, and priority
changes immediately. Preserve these rules across compaction without reviving canceled work.
```

These are agent instructions. They do not intercept messages, force background execution, provide scheduling, or override higher-priority policies. Local installations apply to hosts that load these files; cloud sessions require their own supported installation.
