# Conversation task state

Conversation: <trusted conversation identifier, or unique local checkpoint path>
Active outcome: <requested result>
Done when: <observable criteria and required verification>
Status: <active / waiting / paused / canceled / complete>
Next unfinished action: <concrete action>
Last verified checkpoint: <result and artifact or command evidence>
Constraints and exclusions: <latest user instructions>
Waiting on: <specific blocker, or none>

## Queue

| Order | User request | Requested outcome | Executable request or idea | Permission still needed |
| --- | --- | --- | --- | --- |

## Latest changes

<Only current corrections, cancellations, or priority changes needed to resume accurately.>

Keep this state private and conversation-scoped. Do not copy chat content into the public skill package.
