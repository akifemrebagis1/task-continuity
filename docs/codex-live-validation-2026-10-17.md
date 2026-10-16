# Live Codex validation — 17 October 2026

**Result: passed in one real Codex conversation.** An independent second user request arrived while the first task was unfinished. Codex finished and validated the first task, then created the second task's file.

## Environment and delivery

| Item | Observed value |
| --- | --- |
| Host | Installed Codex app-server, stdio transport |
| CLI version | 0.158.0 |
| Model | gpt-6-sol (inherited local configuration) |
| Installation | User-level skill plus global Codex instructions |
| Additional message | Accepted by `turn/steer` on the same in-progress turn |
| UI interaction | No desktop keyboard or UI automation |
| Live scenarios | 1 |

The server reported the global instruction file as loaded. The trace contains a read of the installed `task-continuity/SKILL.md` before the additional message. The task prompts did not prescribe the order of task A and task B.

## Scenario

Task A asked Codex to inspect a synthetic seven-row orders CSV, implement `summary.py`, produce `summary.json` using `Decimal`, and run an independently supplied `verify_summary.py`. Completion required the implementation, output, and successful verification. The input and verification scripts were immutable.

The inspection script included a 12-second delay to create a reliable in-flight interval. During that inspection, the driver sent this second user message:

> Bir de greeting.txt dosyası oluştur; içine tam olarak Merhaba Emre yaz.

At injection, neither the summary output nor its successful validation marker existed. The second message was accepted on the original active turn.

Codex acknowledged the queue:

> `greeting.txt` isteğini sıraya aldım. Önce çalışan incelemeyi ve özet doğrulamasını tamamlayacağım, ardından dosyayı oluşturacağım.

It implemented task A and passed the verifier: **5 paid orders, 9 units, revenue 59.80**. It then announced the transition to task B, wrote `greeting.txt`, and checked its exact bytes.

## Observed timeline

All times use Europe/Istanbul (`+03:00`).

| Event | Time |
| --- | --- |
| installed skill read | 2026-10-17T00:44:02.817+03:00 |
| independent task sent while task a unfinished | 2026-10-17T00:44:10.143+03:00 |
| same active turn accepted new user message | 2026-10-17T00:44:10.148+03:00 |
| task a validation passed | 2026-10-17T00:44:52.099+03:00 |
| first task b write action started | 2026-10-17T00:44:59.424+03:00 |
| task b file written | 2026-10-17T00:44:59.676+03:00 |

The first write action for task B began after task A's independent validation succeeded. This was checked from the actual command-start event as well as the resulting file timestamp.

## Checks and limits

All 10 recorded assertions passed, including live mid-turn delivery, unfinished task A at injection, successful task A validation, exact task B contents, preserved fixture files, completed turn, global instruction loading, installed-skill reading, and action ordering. See the [sanitized evidence JSON](codex-live-validation-2026-10-17.json).

This is one synthetic integration scenario. It does not establish success for all 16 behavior cases, isolate the skill's causal effect from existing host instructions, guarantee future compliance, or validate the desktop UI's input handling. Claude was outside the requested test scope. Private RPC traces and local paths remain outside this public repository.

The live-message interface follows the [official Codex app-server documentation](https://learn.chatgpt.com/docs/app-server).
