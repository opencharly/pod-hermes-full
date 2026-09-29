# pod-hermes-full

The `hermes-full` candy of the OpenCharly candy library, as a standalone repo
(kind-prefixed naming). It is a pure meta-composition: a standalone Hermes
AI-agent image with the AI coding CLIs and the developer / DevOps toolchains.

## What it provides

`hermes-full` installs nothing of its own — it composes the hermes agent, four AI
coding CLIs, the dev-tools and devops-tools toolchains, and tmux. Its observable
effect is that every composed candy's key artifact coexists in ONE image, and the
hermes agent comes up as a supervised service when the box is deployed. Dropping
any candy from the list makes the matching check fail.

| Composed candy | What it contributes |
|---|---|
| `pod-hermes` | The Hermes agent (pixi CLI + `hermes-entrypoint` + supervised service) |
| `layer-claude-code` | Anthropic Claude Code CLI (`/usr/local/bin/claude`) |
| `layer-codex` | OpenAI Codex CLI (`~/.npm-global/bin/codex`) |
| `layer-gemini` | Google Gemini CLI (`~/.npm-global/bin/gemini`) |
| `layer-forgecode` | Forge CLI (`~/.npm-global/bin/forge`) |
| `layer-dev-tools` | Developer CLI toolchain (`rg`, `nvim`, …) |
| `layer-devops-tools` | DevOps CLI toolchain (`aws`, `tofu`, `jq`, …) |
| `layer-tmux` | The tmux multiplexer |

## How to use it

Compose it into a box (the `hermes` skill's box example composes `hermes-full`
plus `agent-forwarding` and `pod-dbus`):

```yaml
hermes:
  base: fedora
  candy:
    - '@github.com/opencharly/layer-agent-forwarding:<tag>'
    - '@github.com/opencharly/pod-hermes-full:<tag>'
    - '@github.com/opencharly/pod-dbus:<tag>'
```

```bash
charly box build hermes
charly config hermes -e OLLAMA_API_KEY=your-key   # or OPENROUTER_API_KEY
charly start hermes
charly shell hermes -c "hermes chat"
```

## Layout

- `charly.yml` — the `hermes-full:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Closest family skill: `/charly-hermes:hermes-full-layer` — the metalayer
  composition details. This candy has no `skill:` entity of its own (recorded on
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291)).
- `/charly-hermes:hermes` — the core agent (env_accept, browser dispatch, LLM
  config).
- `/charly-coder:claude-code` / `/charly-coder:codex` / `/charly-coder:gemini` /
  `/charly-coder:forgecode` — the composed AI coding CLIs.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
