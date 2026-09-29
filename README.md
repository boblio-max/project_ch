# Project CopyHatch

a **FULL opencode mech suit** — an opencode-style coding TUI reskinned as a mech pilot HUD, written *entirely* in the Origin programming language. one `main.or` (328 lines), zero dependencies beyond the Origin runtime. it's half parody, half proof that Origin can carry a real interactive app.

## how it actually works

- `main.or` — the whole app: boot sequence, HUD frame rendering, command dispatch loop
- `/suit` `/tools` `/model` — mech status, tool loadout, model picker (all flavor, all glorious)
- `/read` `/write` `/bash` `/web` — file + shell + web ops from inside the HUD
- `/pilot` — the killer feature: shells out to the real `opencode` CLI, so the parody HUD drives an actual coding agent
- `manifest.toml` (`copyhatch v0.1.0`, `entry=main.or`, `vm` backend), `config/` for settings, `tests/` for checks

```bash
origin main.or   # Origin 1.7.27+, optional: opencode CLI for /pilot
```

## stack

pure Origin (`.or`), Origin VM backend, optional `opencode` npm package. the joke is the UI — the engineering is real.
