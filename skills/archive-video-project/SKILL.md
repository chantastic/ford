---
name: archive-video-project
description: Archive a finished video project to a user-configured Google Drive destination. Use when the user wants to archive, back up, or hand off a completed video project (final exports, thumbnail, captions, and original source recordings), or check an existing archive. Inventories only user-authorized folders, proposes a source-to-archive mapping with file sizes, missing deliverables, and collisions, copies only after approval, then verifies the copy and its remote sync before anything else happens. Keeps original recordings unchanged, never deletes from the synced archive, never changes sharing, and needs explicit per-file approval before removing any local original.
---

# Archive video project

> Draft skill. Review it before relying on it.

Copy a finished video project into an archive folder that is easy to retrieve later. Prove that the archive is complete and stored remotely, and only then talk about cleaning up local files.

## Ground rules

- **Authorized paths only.** Read only the local project root and the archive destination that the user names for this task. Do not browse other folders, accounts, or drives.
- **Copy, never move.** The archive is a copy. Never use "move" from the working folder to the archive, and never delete anything as part of copying.
- **Originals stay as they are.** Archive original source recordings at full size, byte for byte. Do not compress, transcode, trim, unzip, or leave out source files unless the user explicitly asks for that specific change.
- **Never delete from the archive.** Once files are in a synced archive folder, deleting them there deletes them from the cloud.
- **Never change sharing.** Do not change permissions, owners, or link sharing on any file or folder. Destinations may already be shared with collaborators (see [Destinations](#destinations)).
- **No credential shortcuts.** Use a Google Drive connector if the host provides one and the user has authorized it, or a Drive folder that is visibly mounted on the user's machine and that the user names. Never read access tokens, browser sessions, cookies, or a sync client's internal databases.
- **Don't invent artifacts.** If a deliverable is missing, report it as missing. Do not render, transcode, or generate a stand-in. A file's type does not prove it is the final version; confirm with the user.
- **Keep private details out of shared places.** Destination links, folder IDs, account names, and local paths belong to this run's configuration only. Do not write them into skills, repos, or other shared files.

## Target layout

Each archived project is one folder named `yyyy-mm-dd-{video-name}`, using the publish date (or the date the user gives) and a short, human-readable kebab-case name. Deliverables sit at the root so they are easy to find. Everything else goes in `src/`. There is no `exports/` folder.

```text
yyyy-mm-dd-{video-name}/
├── yyyy-mm-dd-{video-name}-youtube.mov        # full-quality master
├── yyyy-mm-dd-{video-name}-x.mp4              # transcode for X
├── yyyy-mm-dd-{video-name}-thumbnail.jpg      # or .png
├── yyyy-mm-dd-{video-name}-transcript.srt     # captions; keep .itt if that is what exists
└── src/
    └── ...                                     # original recordings and supporting assets
```

`src/` holds original recordings (including recorder project bundles and archives, such as a Screen Studio ZIP) and supporting video, sound effects, music, and images. Inside `src/`, keep the relative layout that project files depend on. See [references/naming-and-layout.md](references/naming-and-layout.md) for naming variants, older archives, and what counts as which deliverable.

## Destinations

The user configures destinations privately at runtime, for example in their own host settings or by telling you during the task. This skill names them by project type only:

| Parameter | Use for |
| --- | --- |
| `{archive-root:educational}` | Educational and announcement videos |
| `{archive-root:event}` | Event projects with external collaborators |

If the destination for the project type is not configured, ask for it. Do not guess or search for it. Before copying into an event destination, remind the user that it may be shared with external collaborators, and get confirmation that everything being copied is appropriate for them to see.

## Workflow

### 1. Confirm scope

Confirm all of the following:

- The local project root.
- The project type, and with it, the destination.
- The video name and date for the folder name.
- Whether the user wants to keep local originals after archiving. Keeping them is a valid choice, and it ends the workflow at step 6.

Then check how the source and destination relate. Decide whether the local project root is itself inside a Drive-synced folder, and whether it is already inside the destination tree. If it is, the "local original" is a Drive-backed copy, and deleting it would delete cloud data. Note this in the manifest and treat that copy as part of the archive.

### 2. Inventory

List every file and folder under the project root, with its relative path, size in bytes, and modification time. Treat app bundles (such as `.screenstudio` or `.fcpbundle`), ZIP archives, and camera-card trees as single units, and record their total sizes. Look at the destination too: whether the project folder already exists there and what it contains.

Read only. Do not open projects in their editing apps, because that can auto-save or relink media.

### 3. Propose the mapping

Fill in [assets/archive-manifest-template.md](assets/archive-manifest-template.md):

- **Deliverables:** for each of the four deliverables, the candidate source file, its size, its archive name, and your confidence. Mark each one as **missing** or **ambiguous** where that applies. For example, when two `.mov` files could be the master, list both and ask.
- **Source:** every original recording and supporting asset, mapped to a path under `src/`.
- **Not archived:** caches, renders, or duplicates you propose to leave out, each with a reason. Leaving out original source requires the user's explicit instruction.
- **Collisions:** any destination path that already exists or that two sources would share. Never overwrite. Propose keeping both or skipping, and let the user decide.
- **Totals:** file count and bytes to copy, compared with the free space at the destination if that is visible.
- **Linked media:** whether project files in `src/` reference media by paths that the new layout will change.

### 4. Get approval for the copy

Present the manifest and the open questions, and revise it until the user explicitly approves the copy. Approving the copy does not approve any deletion.

### 5. Copy

Copy exactly what the approved manifest lists, and nothing else.

- Use a copy method that keeps bundles and file metadata intact, and that refuses to overwrite.
- Rename only the archive copies of deliverables, never the source files.
- If something fails or the destination changed since the inventory, stop and report.

### 6. Verify

Follow [references/verification.md](references/verification.md). Record the evidence for each file in the manifest:

1. **Copy verified:** the destination has every manifest item, with matching byte counts. Use checksums when the user wants the extra assurance and the files are local on both sides.
2. **Remote persistence verified:** evidence from the server side that Drive stores the files. Seeing a file in a mounted Drive folder does not prove it has uploaded, and local checksums do not prove anything about the cloud.

If remote persistence cannot be confirmed, mark the archive **unverified**. Unverified archives block all cleanup.

### 7. Optional local cleanup

Only if the user wants it, and only when every file is **remote-verified**:

- List the exact local paths you propose to remove, with sizes. Leave out anything inside a Drive-synced tree or inside the archive.
- Get explicit approval for that exact list. Agreeing to an archiving policy earlier does not count as approval to delete.
- Prefer recoverable removal, such as moving to the operating system's trash, when the host actually supports it. If it doesn't, say that removal would be permanent, and let the user decide or do it themselves.
- Report what was removed and what was kept.

## Checking existing archives

When asked to check an archive the user made earlier, classify what is there against the target layout and report how it differs (see the naming reference). Do not rename or reorganize older archives unless the user asks for that specific change.
