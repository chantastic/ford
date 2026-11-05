---
name: archive-video-project
description: Archive a finished video project into a user-configured Google Drive destination. Use when the user wants to archive, back up, or hand off a completed video project (final exports, thumbnail, captions, and original source recordings), or check an existing archive. Inventories only user-authorized folders, proposes a source-to-archive mapping with file sizes, missing deliverables, and collisions, and after approval moves the listed files by copying, verifying the transferred contents, and only then removing those exact originals. Reports cloud sync separately from the local transfer. Keeps original recordings unchanged, never overwrites, never deletes from the archive or a Drive-synced tree, and never changes sharing.
---

# Archive video project

> Draft skill. Review it before relying on it.

Move a finished video project into an archive folder that is easy to retrieve later. The move is safe: copy, verify the copied contents, then remove only the exact originals that the approved request covers. Cloud sync is reported separately and does not gate the local cleanup.

## Ground rules

- **Authorized paths only.** Read only the local project root and the archive destination that the user names for this task. Do not browse other folders, accounts, or drives.
- **Approval covers exact paths only.** Approving the archive manifest authorizes moving the exact original paths it lists, and nothing else. Do not widen it to other files, folders, or projects. A general policy statement is not approval for any specific archive; each archive needs its own approved manifest.
- **Verify before removing.** Remove an original only after its transferred copy has been verified. If any verification fails, remove nothing.
- **Originals stay as they are.** Archive original source recordings at full size, byte for byte. Do not compress, transcode, trim, unzip, or leave out source files unless the user explicitly asks for that specific change.
- **Never overwrite.** Existing destination files are collisions. Resolve each one with the user before transferring.
- **Never delete from the archive or a Drive-synced tree.** Deleting there deletes from the cloud, and the change propagates.
- **Never change sharing.** Do not change permissions, owners, or link sharing on any file or folder. Destinations may already be shared with collaborators (see [Destinations](#destinations)).
- **No credential shortcuts.** Use a Google Drive connector if the host provides one and the user has authorized it, or a Drive folder that is visibly mounted on the user's machine and that the user names. Never read access tokens, browser sessions, cookies, or a sync client's internal databases.
- **Don't invent artifacts.** If a deliverable is missing, report it as missing. Do not render, transcode, or generate a stand-in. A file's type does not prove it is the final version; confirm with the user.
- **Keep private details out of shared places.** Destination links, folder IDs, account names, and local paths belong to this run's configuration only. Do not write them into skills, repos, or other shared files.

## Target layout

Each archived project is one folder named `yyyy-mm-dd-{video-name}`, using the publish date (or the date the user gives) and a short, human-readable kebab-case name. Deliverables sit at the root so they are easy to find. Everything else goes in `source/`. There is no `exports/` folder.

```text
yyyy-mm-dd-{video-name}/
├── yyyy-mm-dd-{video-name}-youtube.mov        # full-quality master
├── yyyy-mm-dd-{video-name}-x.mp4              # transcode for X
├── yyyy-mm-dd-{video-name}-thumbnail.jpg      # or .png
├── yyyy-mm-dd-{video-name}-transcript.srt     # captions; keep .itt if that is what exists
└── source/
    └── ...                                     # original recordings and supporting assets
```

`source/` holds original recordings (including recorder project bundles and archives, such as a Screen Studio ZIP) and supporting video, sound effects, music, and images. Inside `source/`, keep the relative layout that project files depend on. See [references/naming-and-layout.md](references/naming-and-layout.md) for naming variants, older archives, and what counts as which deliverable.

## Destinations

The user configures destinations privately at runtime, for example in their own host settings or by telling you during the task. This skill names them by project type only:

| Parameter | Use for |
| --- | --- |
| `{archive-root:educational}` | Educational and announcement videos |
| `{archive-root:event}` | Event projects with external collaborators |

If the destination for the project type is not configured, ask for it. Do not guess or search for it. Before transferring into an event destination, remind the user that it may be shared with external collaborators, and get confirmation that everything being transferred is appropriate for them to see.

## Workflow

### 1. Confirm scope

Confirm all of the following:

- The local project root.
- The project type, and with it, the destination.
- The video name and date for the folder name.
- The mode. **Move** is the default: transfer, verify, then remove the listed originals. **Copy** keeps the local originals; the user can always choose it.

Then check how the source and destination relate. Decide whether the local project root is itself inside a Drive-synced folder, and whether it is already inside the destination tree. If it is, its files are Drive-backed copies. Removing them would delete cloud data, so do not remove them. Report this, and treat the files as copy-only unless the user gives a separate, explicit instruction for those exact paths.

### 2. Inventory

List every file and folder under the project root, with its relative path, size in bytes, and modification time. Treat app bundles (such as `.screenstudio` or `.fcpbundle`), ZIP archives, and camera-card trees as single units, and record both their total sizes and their file counts. Look at the destination too: whether the project folder already exists there and what it contains.

Read only. Do not open projects in their editing apps, because that can auto-save or relink media.

### 3. Propose the mapping

Fill in [assets/archive-manifest-template.md](assets/archive-manifest-template.md):

- **Deliverables:** for each of the four deliverables, the candidate source file, its size, its archive name, and your confidence. Mark each one as **missing** or **ambiguous** where that applies. For example, when two `.mov` files could be the master, list both and ask.
- **Source:** every original recording and supporting asset, mapped to a path under `source/`.
- **Not archived:** caches, renders, or duplicates you propose to leave out, each with a reason. These are not moved and are left in place; they are never removed by this skill. Leaving out original source requires the user's explicit instruction.
- **Collisions:** any destination path that already exists or that two sources would share. Never overwrite. Propose keeping both or skipping, and let the user decide.
- **Totals:** file count and bytes to transfer, compared with the free space at the destination if that is visible.
- **Linked media:** whether project files in `source/` reference media by paths that the new layout will change.
- **Removal list (move mode):** the exact original paths that will be removed after verification. This is the same set as the transferred items, minus anything inside a Drive-synced tree or the archive.

### 4. Get approval

Present the manifest and the open questions, and revise it until the user explicitly approves it. In move mode, approving the manifest authorizes removing the listed originals after verification. No second approval is needed for those exact paths. Anything not on the removal list needs its own explicit instruction.

### 5. Transfer

Transfer exactly what the approved manifest lists, and nothing else.

- Treat a move into a Drive-mounted folder or onto another volume as three steps: copy, verify, then remove. Do this even if the operating system offers a one-step move.
- Use a copy method that keeps bundles, file metadata, and relative paths intact, and that refuses to overwrite.
- Rename only the archive copies of deliverables, never the source files.
- If something fails, or the destination has changed since the inventory, stop, remove nothing, and report.

### 6. Verify the transfer

Follow [references/verification.md](references/verification.md). For every transferred item, record:

- **Byte count:** the destination copy matches the source, and for folders and bundles, the file count matches too.
- **Checksum:** the destination contents, read back, match the source. This is required before removing an original. Seeing a file in a Drive folder, or an online-only placeholder, does not prove the transfer is complete.

If any item fails or cannot be checked, the transfer is **not verified**. Remove nothing, and report which items failed.

### 7. Remove the originals (move mode)

Only when every item is verified:

- Remove only the exact paths on the approved removal list. Never remove anything inside the archive, inside a Drive-synced tree, or outside the list.
- Prefer recoverable removal, such as moving to the operating system's trash, when the host actually supports it. If only permanent removal is available, say so before acting. Trashed files still use disk space until the trash is emptied.
- Leave emptied folders and the project root in place unless the user asks otherwise.
- If a removal fails partway, stop and report exactly what was and wasn't removed.

### 8. Report

Report the local result and the cloud state as separate lines:

- **Archived locally:** the transfer is verified, originals are removed or kept, and any skipped or left-in-place items are listed.
- **Cloud status:** **sync pending / unverified** unless there is server-side evidence (see the verification reference), in which case it is **cloud backed up**. Never report "backed up" from local evidence alone.

When sync is pending and originals have been removed, the archive exists only on this machine until upload completes. Tell the user, and suggest they not disconnect the Drive account or clear its local cache until the upload finishes.

## Checking existing archives

When asked to check an archive the user made earlier, classify what is there against the target layout and report how it differs (see the naming reference). Do not rename or reorganize older archives unless the user asks for that specific change.
