# AGENTS.md

## Toolchain (mise)

This is a GitHub profile README repo (README + one workflow). It uses [**mise**](https://mise.jdx.dev) to pin linters, expose tasks, and wire git hooks. `mise.toml` is the source of truth. Don't install tools by hand or add ad-hoc scripts; add a mise tool or task instead.

**Setup** (once, and per new worktree): `mise trust && mise run setup`.

**Run via mise.** Run `mise run check` before you call work done. Discover the rest with `mise tasks` / `mise run <task> --help`:

```sh
mise run check          # all linters/formatters/validators (alias: lint); add --fix to auto-fix
mise run test           # no-op placeholder (no app tests)
mise tasks              # discover every task
```

Prefer `mise run <task>` over calling the tool directly, so local, hooks, and CI stay in sync.

The profile `README.md` is the public GitHub profile page — keep toolchain docs here, not in that file.

## Git hooks (hk)

Commits run [hk](https://hk.jdx.dev), the same `check` CI runs, to format and lint staged files. Fix failures with `mise run check --fix`. Don't disable steps to push a commit through; `git commit --no-verify` skips hooks for a WIP commit.

## Extending the setup

Changing tools, tasks, env, or hooks? Edit the config, don't bolt on scripts, then run `mise run check`. Where things live:

- **`mise.toml`**: the source of truth for `[tools]`, `[tasks]`, `[env]`/`[vars]`, `[settings]`, and `[hooks]`.
- **`mise.lock`**: resolved versions plus checksums. Commit it; regenerate with `mise install` after a `[tools]` change.
- **`.mise/`**: project-local state (gitignored), like the setup stamp the `setup`/`enter` hooks read.
- **`hk.pkl`**: the pre-commit and `check` pipeline (linters and formatters, in Pkl). Add or edit a lint step here.

For tool, task, and hook syntax, see the [mise](https://mise.jdx.dev) and [hk](https://hk.jdx.dev) docs.
