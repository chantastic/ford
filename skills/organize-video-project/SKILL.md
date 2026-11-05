---
name: organize-video-project
description: Inventory a video project folder and propose a reorganization as a dry-run manifest, without moving anything. Use when the user asks to inventory, audit, clean up, restructure, or standardize recurring video project folders (DaVinci Resolve, Premiere Pro, Final Cut Pro, After Effects, screen-recording tools, camera-card offloads). Identifies the editing application and its media dependencies, classifies originals, proxies, audio, graphics, project files, exports, and caches, and produces a source-to-destination plan with reasons, collisions, and a rollback strategy for explicit user approval. Does not move, rename, copy, or delete files.
---

# Organize video project

Turn a messy video project folder into a reviewable reorganization plan. The deliverable is an inventory, a proposed layout, and a dry-run manifest. Nothing on disk changes.

## Ground rules

- **Authorized roots only.** Work only inside folders the user names explicitly for this task. Ask for the root path if it was not given. Do not scan home directories, whole volumes, or sibling folders "for context."
- **Read-only.** Use listing and metadata commands only (for example `ls`, `find`, `stat`, `du`, and `ffprobe` or `mediainfo` if they are already installed). Do not move, rename, copy, delete, touch, chmod, or create files inside the root. Write the plan in the conversation, or in a file outside the root if the user names one.
- **Do not open projects in their editing application.** Opening a project can auto-save, re-link media, or rebuild caches. Inspect project files as files only.
- **No execution in this skill.** This skill ends with a plan the user has approved. Carrying out the plan is a separate request (see [After approval](#after-approval)).
- **Keep private details private.** Do not copy file names, client names, or paths from the user's project into shared documents, skill files, or examples unless the user asks for that.

## Workflow

### 1. Confirm scope

Before reading anything, confirm:

- The exact root path or paths to inspect.
- Whether the project is finished (archive), in progress (editing), or new (setup).
- Which editing application the user believes the project uses, if they know.
- Whether any files must stay where they are, such as media shared with other projects or a folder synced by another tool.

### 2. Inventory first

Build the inventory before proposing anything. For each top-level folder and each notable file group, record:

- Relative path, file count, total size, and the most common extensions.
- Most recent modification time. This helps tell active work from stale renders.
- Structural markers: camera-card folder trees, application bundles or packages, sidecar files, and checksum files (`.mhl`, `.md5`).

On large trees, summarize per folder rather than listing every file. Use [references/media-signals.md](references/media-signals.md) to recognize file types.

### 3. Identify the editing application and its dependencies

Determine which application(s) own the project from the project files present (see the signals reference). Then work out what the project files depend on:

- **Linked media paths.** Most editing applications store absolute or root-relative paths to media. Moving media without re-linking in the application breaks the project. Record which folders the project files appear to reference. If the format can't be read safely, say the dependency is unknown and do not guess.
- **Application-managed folders.** Library bundles, project packages, managed media folders, and cache locations belong to the application. Treat their internals as opaque and do not propose reorganizing inside them.
- **Cross-application links**, such as an After Effects composition used inside a Premiere project (Dynamic Link) or round-trip exports between an editor and an audio tool.

### 4. Classify

Put every file group into one of these buckets, noting your confidence and the evidence for each:

| Bucket | Examples | Default handling |
| --- | --- | --- |
| Originals | Camera-card offloads, screen recordings, field audio | Preserve exactly; never rename inside card structures |
| Proxies / optimized media | Proxy and optimized-media folders, transcodes | Regenerable, but linked; keep next to what the app expects |
| Audio | Music, VO, sound effects, stems, audio-tool sessions | Separate source audio from mixes and stems |
| Graphics | Stills, logos, lower thirds, motion-graphics projects | Separate source design files from rendered assets |
| Project files | Editing projects, libraries, backups, auto-saves | Keep with the folders they reference |
| Exports | Masters, platform deliverables, review cuts, captions | Distinguish final deliverables from drafts |
| Caches / renders | Render files, media cache, peak files, waveforms | Regenerable; flag for possible exclusion, never delete in this skill |
| Unknown | Anything you cannot classify confidently | Leave in place and ask the user |

### 5. Propose a layout based on the inventory

Propose a target layout that fits what you found. Do not impose a universal scheme. Prefer the smallest change that makes the project understandable:

- Reuse the user's existing folder names and conventions where they work.
- Keep camera-card trees, application bundles, and checksum files intact as units.
- Keep project files and the media they link to in the same relative arrangement unless the user plans to re-link.
- Note which moves the editing application will need to re-link, and which are invisible to it.

An example shape, offered only as a starting point for discussion:

```text
example-project/
├── 01-project/      # editing project files and backups
├── 02-originals/    # camera cards and recordings, structure preserved
├── 03-audio/
├── 04-graphics/
├── 05-proxies/      # only if the app is configured to look here
└── 06-exports/
```

### 6. Write the dry-run manifest

Use the template in [assets/dry-run-manifest-template.md](assets/dry-run-manifest-template.md). The manifest must include:

- One row per proposed operation: source, destination, operation type (move or rename), and the reason.
- **Collisions:** any destination that already exists or that two sources would both target, with a proposed resolution. Never overwrite silently.
- **Link impact:** which project files will need media re-linked after each move.
- **Excluded items:** what is deliberately left in place and why.
- **Deletions:** list candidates for deletion (such as regenerable caches) in a separate section, marked as requiring separate explicit approval. Do not mix them in with moves.
- **Rollback strategy:** the reverse of every operation (destination back to source), a recommendation to back up or snapshot first, and how to verify the result (for example, file count, total size, and checksums for originals).

### 7. Review with the user

Present the inventory summary, the proposed layout, and the manifest. Point out open questions and low-confidence classifications. Revise until the user explicitly approves the plan. Approval of the layout is not approval to execute.

## After approval

This skill does not execute the plan. If the user later asks to carry it out, that is a separate, explicit request, and it should follow these rules:

- Get explicit confirmation of the final manifest immediately before acting.
- Make sure a verified backup exists first.
- Never delete or overwrite anything without separate, item-level approval.
- Work in small, reversible batches and verify each one before continuing.
- Re-link media in the editing application as the manifest describes.

## Refining this skill

The guidance here is general. When you examine a real, user-authorized sample project, record recurring patterns, such as folder names the user actually uses, the applications in their workflow, and edge cases that tripped up classification. Then propose edits to this skill in generic terms that do not include private paths or project names.
