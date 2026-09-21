# Project CopyHatch

A **FULL opencode mech suit** — an opencode-style TUI in a mech HUD,
written entirely in the [Origin programming language](https://github.com/boblio-max/origin)
(`.or` files, ~250 lines, zero dependencies beyond the Origin runtime).

```
+===============================================+
|  C O P Y H A T C H   v0.1.0                   |
|  FULL opencode mech suit -- Origin-built TUI   |
+-- COPYHATCH HUD ---------------------------------+
| REACTOR [########--] 82%  ARMOR 100% |
| MODEL muse-spark  MISSIONS 0            |
+-------------------------------------------------+
copyhatch/muse-spark> /help
```

## What it is

CopyHatch spins up a full opencode-style agent loop — prompt, tools,
models, session log — reskinned as piloting a mech suit. Every tool call
costs reactor power and wears armor; chatting recharges the core. When you
want the real thing, `/pilot` suit-links straight into the actual opencode
TUI and drops you back in the suit when you quit.

## Requirements

| Need | Notes |
|---|---|
| Origin 1.7.27+ | `pip install origin-or`, then `origin main.or` (VM backend is default; `origin i main.or` also works) |
| opencode (optional) | Only for `/pilot` — `npm i -g opencode` |

## Run

```powershell
cd project_ch
origin main.or
```

## REPL plates

| Command | Effect |
|---|---|
| `/help` | command list |
| `/suit` | HUD status (reactor / armor / model / missions) |
| `/tools` | FULL tool manifest — every plate live |
| `/model` | swap pilot model (`muse-spark`, `opus-mech`, `sonnet-scout`) |
| `/read` | read a file (prompts for path) |
| `/write` | write a file (prompts for path, then text) |
| `/bash` | run a shell command |
| `/web` | fetch a URL (first bytes) |
| `/log` | show the sealed session log |
| `/clear` | wipe the visor |
| `/repair` | restore armor to 100% (costs 10 reactor) |
| `/pilot` | suit-link into the REAL opencode TUI |
| `/exit` | eject (quit) |

Anything else is mission traffic: the suit answers, seals both sides to
`session.log`, and recharges +2 reactor.

## Suit mechanics

- **Reactor** starts at 82%. Tools drain it (`bash` 8, `web` 6, `write` 5,
  `read` 3); hitting 0 triggers an emergency trickle charge.
- **Armor** starts at 100%. `bash`/`write` wear it by 1; `/repair` restores
  it for 10 reactor (refused below 10).
- **Missions** count every action and rotate the suit's replies.

## Project structure

```
project_ch/
  main.or              # the whole suit: [CORE] [TUI] [TOOLS] [SESSION]
  manifest.toml        # Origin package manifest
  config/opencode.json # companion FULL config for real opencode
  tests/drive.txt      # piped drive-test script (non-interactive)
  README.md
```

One suit file is deliberate: Origin 1.7.x inlines only stdlib imports, so
sibling `.or` modules don't resolve yet. The plates are delimited sections
inside `main.or` and split into separate modules once local imports land
in v1.8. Verified class-free on purpose — class construction is broken in
the 1.7.27 VM backend (only `origin i` runs classes).

## Drive test

```powershell
Get-Content tests/drive.txt | origin main.or
```

Covers help, HUD, tool manifest, model swap, mission reply, bash, read,
write, log, and eject. `/pilot` and `/web` are manual-only.

## Roadmap

- [ ] Split plates into `suit.or` / `tui.or` / `tools.or` / `session.or` on Origin v1.8 local imports
- [ ] Rich HUD via `py{}` + `rich` (still Origin — `py{}` blocks are idiomatic)
- [ ] `/model` wired to real opencode model switching through the suit-link
- [ ] Armor damage model tied to real tool exit codes

## License

MIT — plate it, fork it, fly it.
