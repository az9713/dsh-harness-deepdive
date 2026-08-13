# deepseek-harness deepdive

An independent, warts-and-all review of **DeepSeek Harness (`dsh`)** — DeepSeek's
open-source agent harness — published here as 8 self-contained HTML pages with
diagrams.

**Read it: https://az9713.github.io/deepseek-harness-deepdive/**

## Which version this reviews

Every claim in these pages was verified against one exact snapshot of upstream.
If you compare them to a newer upstream, expect drift: the project moves at
roughly 104 commits a day and states that it makes compatibility-breaking
changes.

| | |
|---|---|
| Upstream repository | https://github.com/deepseek-ai/deepseek-harness |
| Commit reviewed | `47f943859bef60e4160492346772ded9b24f765a` (short `47f943859b`) |
| Commit date | 2026-08-13 |
| Commit subject | `Merge pull request #2519 from deepseek-harness/feat/npm-public` |
| Package version | `0.1.0-rc.5` |
| Review written | 2026-08-13 |

Claims are cited as `path/to/file.ts:42`. Those line numbers are valid at commit
`47f943859b` and nowhere else. Re-verify before relying on any of them.

## The pages

Start at **[index.html](index.html)**. Every page carries a navigation bar to
the other seven.

| Page | What it answers |
|---|---|
| `index.html` | The headline, the one-screen verdict, how to read the review. |
| `what-is-dsh.html` | What ships, what happens when you run `dsh web`, where state lives on disk. |
| `architecture.html` | The plugin model, the turn loop end to end, the session log, the host/client split. |
| `capabilities-and-security.html` | Tools, seams, the sandbox — what is enforced and what is only advisory. |
| `how-they-build-it.html` | Velocity, agent-driven development, the `.agents/` tree, testing, CI, releases. |
| `strengths.html` | The ranked list of things worth stealing. |
| `weaknesses.html` | The ranked list of warts, each with evidence. |
| `verdict.html` | Adopt, study, or wait. |

## Diagrams

Eight hand-drawn inline SVG diagrams cover the boot sequence, the browser/server
channels, the session-log invariant, the turn loop, the one dependency cycle,
the security boundary, the tool-execution pipeline, and the cost of one change.

Three colours carry meaning throughout:

- **teal** — enforced, or a design worth copying
- **orange** — a risk, or a thing outside the security boundary
- **dashed border** — advisory only, not enforced

## How to view

Online, at **https://az9713.github.io/deepseek-harness-deepdive/**.

Or locally. The pages are self-contained — the CSS and the SVG diagrams are
inside each file and nothing loads from the network — so a clone works with no
build step:

```
git clone https://github.com/az9713/deepseek-harness-deepdive.git
cd deepseek-harness-deepdive
start index.html      # Windows; use "open" on macOS, "xdg-open" on Linux
```

Note that GitHub's file view shows HTML source rather than the rendered page.
Use the Pages link above, or a local clone, to read it as intended.

## Status

Independent and unaffiliated. This is not a DeepSeek publication, and it is not
endorsed by the deepseek-harness authors. Upstream accepts no external pull
requests, so nothing here was submitted to them.
