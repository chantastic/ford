# ford

Workplace agent skills for my developer educator video production job.

Ford is a small, portable, instruction-only plugin. It packages [Agent Skills](https://agentskills.io/specification) in the [Agent Plugins](https://agent-plugins.org) format, with a compatibility manifest for Claude Code. It contains no hooks, MCP servers, or scripts.

> **Status: draft.** The skill below has not been reviewed against a real archive, and the plugin has not been loaded in any host yet. See [Limitations](#limitations).

## Skills

| Skill | What it does |
| --- | --- |
| [`archive-video-project`](skills/archive-video-project/SKILL.md) | Archives a finished video project (final exports, thumbnail, captions, and original source) to a Google Drive destination that you configure. It inventories first, proposes a source-to-archive mapping for approval, copies without moving, and verifies both the copy and the remote sync. Removing a local original needs your explicit approval for that exact file, and the skill never deletes anything from the archive. |

## Archive layout

The skill proposes this layout for new archives. Existing archives vary, and the skill reports how they differ rather than renaming them.

```text
yyyy-mm-dd-{video-name}/
├── yyyy-mm-dd-{video-name}-youtube.mov      # full-quality master
├── yyyy-mm-dd-{video-name}-x.mp4            # transcode for X
├── yyyy-mm-dd-{video-name}-thumbnail.jpg    # or .png
├── yyyy-mm-dd-{video-name}-transcript.srt   # or .itt
└── source/                                  # original recordings and supporting assets, unchanged
```

## Package structure

```text
ford/
├── plugin.json                    # Agent Plugins 1.0.0 manifest (portable)
├── .claude-plugin/
│   └── plugin.json                # Claude Code compatibility manifest
├── skills/                        # shared by every host; one copy of each skill
│   └── archive-video-project/
│       ├── SKILL.md
│       ├── references/
│       │   ├── naming-and-layout.md   # deliverables, source/, and naming variants
│       │   └── verification.md        # copy vs. remote evidence, synced-tree safety
│       └── assets/
│           └── archive-manifest-template.md
└── README.md
```

When you bump the version, change it in both `plugin.json` and `.claude-plugin/plugin.json`.

## Usage

Name the folder you are authorizing and the project type:

- "Archive the project in `{local-project-root}` as an educational video. Show me the manifest first."
- "Check the existing archive at `{archive-root:educational}/yyyy-mm-dd-{video-name}` against the layout. Don't change anything."
- "Is the archive for yyyy-mm-dd-{video-name} fully synced? If you can't confirm it, say so."

The skill stops for your approval twice: once before it copies anything, and again, per file, before it removes any local original. You can always choose to keep local originals.

Hosts decide on their own when to activate a skill, so activation isn't guaranteed. If it doesn't trigger, mention the skill by name.

## Configuration (not secrets, not in this repo)

The skill refers to destinations only by placeholders:

| Placeholder | Project type |
| --- | --- |
| `{archive-root:educational}` | Educational and announcement videos |
| `{archive-root:event}` | Event projects with external collaborators |

Supply the real destinations privately at runtime. Either tell the agent during the task, or put them in your own host-level instructions or memory, outside this repository. A destination is configuration, not a credential: the skill needs a folder location, never a token or password. Do not commit destination links, folder IDs, account names, or local paths to this repo or its PRs.

For access, the skill uses either a Google Drive connector that the host provides and you authorize, or a Drive folder visibly mounted on your machine that you name. It never reads tokens, browser sessions, or sync-client databases, and it never changes sharing settings.

## Limitations

- **Draft:** written from an agreed workflow, not yet checked against a real project. Expect revisions.
- **Remote verification depends on the host.** Without a Drive connector that reports file size and checksum, the skill relies on what you can see in Drive. If it can't confirm the sync, it marks the archive *unverified* and blocks cleanup.
- **No automation.** The skill doesn't render, transcode, or generate missing deliverables, and it ships no scripts. The agent performs the copy with the host's own file tools after you approve.
- **Recoverable deletion isn't guaranteed.** The skill uses the OS trash only when the host supports it, and otherwise says that removal is permanent.

## Evolving the skill

1. Chan authorizes one finished sample project and its destination.
2. Run the skill up to the manifest and review it together.
3. Record what was wrong or missing, in generic terms only.
4. Update `SKILL.md` or the references, re-validate, bump both manifest versions, and open a PR.

## Installation by host

"Documented" means the host's official docs describe this layout. "Tested" means someone loaded this plugin in that host and saw the skill. No host has been load-tested yet; only the static validation below has run. There is no single install command that works everywhere, and no marketplace catalog ships in this repository.

| Host | Documented support | Tested | Local testing path |
| --- | --- | --- | --- |
| Cursor | Yes: root `plugin.json` (Agent Plugins) | No | Copy the repo into `~/.cursor/plugins/local/ford`, then run **Developer: Reload Window**. A symlink to a checkout elsewhere is skipped. Enterprise admins can disable local plugins. Invoke with `/archive-video-project`. |
| Claude Code | Yes: `.claude-plugin/plugin.json` plus root `skills/` | No | `claude --plugin-dir /path/to/ford` (this session only). Skills are namespaced, e.g. `ford:archive-video-project`. |
| Codex / ChatGPT | Yes: root `plugin.json` (Agent Plugins) | No | Codex installs plugins from a marketplace catalog. This repo doesn't include one yet, and the documented personal catalog lives under `~/.agents`, which is managed separately from this repo. Decide where a catalog should live before you test. |
| Other Agent Skills clients | Skill folder follows the Agent Skills spec | No | Follow that client's instructions for adding a skill folder. |

Sources: [Cursor plugins](https://cursor.com/docs/plugins) and [reference](https://cursor.com/docs/reference/plugins), [Claude Code plugins reference](https://code.claude.com/docs/en/plugins-reference), [Codex plugins](https://developers.openai.com/codex/plugins/build) and [skills](https://developers.openai.com/codex/skills), [Agent Plugins authoring guide](https://agent-plugins.org/plugin-authors/build-an-agent-plugin).

## Validation

Run from the repo root:

```bash
# Agent Skills frontmatter and naming (pip install skills-ref; provides `agentskills`)
agentskills validate skills/archive-video-project

# Claude Code manifest, paths, and skills (Claude Code CLI)
claude plugin validate --strict .
```

Also validate `plugin.json` against [the Agent Plugins 1.0.0 schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json) with any JSON Schema draft 2020-12 validator, and check that no private destinations, folder IDs, account names, or real local paths appear in the repo.

## License

No license has been chosen yet.
