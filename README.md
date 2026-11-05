# ford

Workplace agent skills for my developer educator video production job.

Ford is a small, portable, instruction-only plugin. It packages [Agent Skills](https://agentskills.io/specification) in the [Agent Plugins](https://agent-plugins.org) format, with a compatibility manifest for Claude Code. It contains no hooks, MCP servers, or scripts. Skills only tell an agent how to work; they never change files on their own.

## Skills

| Skill | What it does |
| --- | --- |
| [`organize-video-project`](skills/organize-video-project/SKILL.md) | Inventories a video project folder you authorize, identifies the editing application and its linked media, classifies files, and proposes a reorganization as a dry-run manifest with reasons, collisions, and rollback. It does not move, rename, copy, or delete anything. |

## Structure

```text
ford/
├── plugin.json                    # Agent Plugins 1.0.0 manifest (portable)
├── .claude-plugin/
│   └── plugin.json                # Claude Code compatibility manifest
├── skills/                        # shared by every host; one copy of each skill
│   └── organize-video-project/
│       ├── SKILL.md
│       ├── references/
│       │   └── media-signals.md   # file-type and application signals
│       └── assets/
│           └── dry-run-manifest-template.md
└── README.md
```

Both manifests point at the same root `skills/` directory. When you bump the version, change it in both `plugin.json` and `.claude-plugin/plugin.json`.

## Usage examples

Once the plugin is loaded, ask in plain language. Always name the folder you are authorizing:

- "Inventory `/Volumes/Example/2026-01-example-tutorial` and tell me what editing app it uses. Don't change anything."
- "Propose a cleaner layout for the project in `~/Videos/example-project` and give me a dry-run manifest."
- "Which folders in `/path/to/example-project` are caches or proxies I could regenerate? Just list them."

Hosts decide on their own when to activate a skill based on its description, so automatic activation isn't guaranteed. If it doesn't trigger, mention the skill by name. Each host has its own explicit syntax; see the table below.

## Evolving the skill

The first version of `organize-video-project` is intentionally general. We refine it like this:

1. Chan picks one sample project and explicitly authorizes its folder.
2. Run the skill against it, read-only, and review the inventory and manifest together.
3. Note what was wrong or missing: misclassified files, application quirks, folder conventions chan already uses, and questions the agent should have asked.
4. Update `SKILL.md` or `references/media-signals.md` with generic versions of those findings. Leave out real paths, project names, client names, and inventories.
5. Re-validate (see below), bump the version in both manifests, and open a PR.

Executing an approved manifest is out of scope for now. If we add it later, it should be a separate skill with explicit approval steps.

## Installation by host

"Documented" means the host's official docs describe this layout. "Tested" means someone loaded this plugin in that host and saw the skill. No host has been load-tested yet; only the static validation below has run. There is no single install command that works everywhere, and no marketplace catalog ships in this repository.

| Host | Documented support | Tested | Local testing path |
| --- | --- | --- | --- |
| Cursor | Yes: root `plugin.json` (Agent Plugins) | No | Copy the repo into `~/.cursor/plugins/local/ford`, then run **Developer: Reload Window**. A symlink to a checkout elsewhere is skipped. Enterprise admins can disable local plugins. Invoke with `/organize-video-project`. |
| Claude Code | Yes: `.claude-plugin/plugin.json` plus root `skills/` | No | `claude --plugin-dir /path/to/ford` (this session only). Skills are namespaced, e.g. `ford:organize-video-project`. |
| Codex / ChatGPT | Yes: root `plugin.json` (Agent Plugins) | No | Codex installs plugins from a marketplace catalog. This repo doesn't include one yet, and the documented personal catalog lives under `~/.agents`, which is managed separately from this repo. Decide where a catalog should live before you test. |
| Other Agent Skills clients | Skill folder follows the Agent Skills spec | No | Follow that client's instructions for adding a skill folder. |

Sources: [Cursor plugins](https://cursor.com/docs/plugins) and [reference](https://cursor.com/docs/reference/plugins), [Claude Code plugins reference](https://code.claude.com/docs/en/plugins-reference), [Codex plugins](https://developers.openai.com/codex/plugins/build) and [skills](https://developers.openai.com/codex/skills), [Agent Plugins authoring guide](https://agent-plugins.org/plugin-authors/build-an-agent-plugin).

## Validation

Run from the repo root:

```bash
# Agent Skills frontmatter and naming (pip install skills-ref; provides `agentskills`)
agentskills validate skills/organize-video-project

# Claude Code manifest, paths, and skills (Claude Code CLI)
claude plugin validate --strict .
```

Also validate `plugin.json` against [the Agent Plugins 1.0.0 schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json) with any JSON Schema draft 2020-12 validator.

## License

No license has been chosen yet.
