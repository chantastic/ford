---
name: archive-video-project
description: Archive a finished video project into the DevEd shared-drive folder by default (or an event destination when the user says so), reusing the project's existing folder if there is one, keeping only the deliverables and critical source. Use when the user wants to archive, back up, slim down, or hand off a completed video project (final exports, thumbnail, captions, and critical source footage), or check an existing archive. Inventories only user-authorized folders, proposes a slimmed source-to-archive mapping with keep and drop lists, transcode list, file sizes, missing deliverables, and collisions, and after approval moves the listed files by copying, verifying the transferred contents, and only then removing those exact originals so no local copy remains. Reports cloud sync separately from the local transfer. Keeps critical source (compressing uncompressed footage to HEVC), drops timelines, intermediate copies, backups, and duplicates, never overwrites, never deletes from the archive or a Drive-synced tree without an explicit instruction for those exact items, and never changes sharing.
---

# Archive video project

> Draft skill. Review it before relying on it.

Move a finished video project into an archive folder that is easy to retrieve later, keeping only the five root deliverables and one `source/` bucket of critical source. The move is safe: copy, verify the copied contents, then remove only the exact originals that the approved request covers. After a verified move, no copy stays in the original location. Cloud sync is reported separately and does not gate the local cleanup.

## Ground rules

- **Authorized paths only.** Read only the local project root and the archive destination that the user names for this task. Do not browse other folders, accounts, or drives.
- **Approval covers exact paths only.** Approving the archive manifest authorizes moving the exact original paths it lists, and nothing else. Do not widen it to other files, folders, or projects. A general policy statement is not approval for any specific archive; each archive needs its own approved manifest.
- **Verify before removing.** Remove an original only after its transferred copy has been verified. If any verification fails, remove nothing.
- **Compress critical source.** Keep already-compressed footage (HEVC, H.264) as is. Transcode uncompressed or huge intermediates (ProRes and similar) to HEVC and drop the original, for all footage, by default. List each transcode in the plan. Do not trim or unzip recordings.
- **Keep only the important stuff.** The five root deliverables, plus critical source and small supporting files in one `source/` bucket. Do not keep timelines (NLE projects and libraries, FCPXML, EDL) or intermediate copies (review cuts, renders, backups, duplicate media, lock, temp, and `.DS_Store` files). The rules and the confirm-before-dropping steps are in [references/what-to-keep.md](references/what-to-keep.md).
- **Move, don't copy.** Once the archive is in place and verified by SHA-256, no copy remains in the original location (for example `~/Downloads`). The original goes to the Mac Trash.
- **Never overwrite.** Existing destination files are collisions. Resolve each one with the user before transferring.
- **Never delete from the archive or a Drive-synced tree on your own.** Deleting there deletes from the cloud, and the change propagates. The one exception is when the user explicitly asks to remove specific items (for example, slimming an existing archive): have Finder delete them, for example `osascript -e 'tell application "Finder" to delete POSIX file "/path/to/item"'`, so they land in Drive's trash and stay recoverable for about 30 days. Do not use `rm` inside the mount.
- **Never change sharing.** Do not change permissions, owners, or link sharing on any file or folder. Destinations may already be shared with collaborators (see [Destinations](#destinations)).
- **No credential shortcuts.** Use a Google Drive connector if the host provides one and the user has authorized it, or a Drive folder that is visibly mounted on the user's machine and that the user names. Never read access tokens, browser sessions, cookies, or a sync client's internal databases.
- **Don't invent artifacts.** If a deliverable is missing, report it as missing. Do not render or generate a stand-in. A file's type does not prove it is the final version; confirm with the user.
- **Keep private details out of shared places.** Destination links, folder IDs, account names, and local paths belong to this run's configuration only. Do not write them into skills, repos, or other shared files.

## Target layout

Each archived project is one folder named `{YYYY-MM-DD} {Project name}` (for example `2026-09-11 Example Project`), using the project or publish date (or the date the user gives) and a short, human-readable project name. Files inside use the kebab-case form `yyyy-mm-dd-{video-name}`. The five deliverables (archive, youtube, social, captions, thumbnail) sit at the root so they are easy to find. See [references/naming-and-layout.md](references/naming-and-layout.md) for what each one is. Everything else goes in `source/`. There is no `exports/` folder.

```text
{YYYY-MM-DD} {Project name}/
├── yyyy-mm-dd-{video-name}-archive.mov        # master export (H.265 these days)
├── yyyy-mm-dd-{video-name}-youtube.mp4        # H.264 YouTube upload
├── yyyy-mm-dd-{video-name}-social.mp4         # H.264, 1080p, for social platforms
├── yyyy-mm-dd-{video-name}-captions.srt       # captions
├── yyyy-mm-dd-{video-name}-thumbnail.jpg      # or .png
└── source/
    └── ...                                     # raw footage and supporting assets
```

`source/` is one bucket for everything that is not a deliverable and is worth keeping: critical source footage (interviews, a-roll, raw b-roll and screen recordings, including recorder archives such as a Screen Studio ZIP) and small supporting files. No sub-taxonomy is needed. See [references/naming-and-layout.md](references/naming-and-layout.md) for naming variants, older archives, and what counts as which deliverable.

## Destinations

The user configures destinations privately at runtime, for example in their own host settings or by telling you during the task. This skill names them by role only:

| Parameter | Use for |
| --- | --- |
| `{archive-root:deved}` | **Default.** Most videos go in the DevEd shared-drive folder, as `{YYYY-MM-DD} {Project name}` |
| `{archive-root:event}` | Exception: event projects, only when the user says so |

Use the DevEd folder unless the user says the project is an event. If the destination is not configured, ask for it. Do not guess where it is, and do not search outside it.

**Look for an existing project folder first.** Before choosing a destination folder, look inside the archive root the user named for a folder that already belongs to this project, matching by project name and date. If one exists, archive into it instead of creating a new one. List its existing contents in the plan. Skip files that are byte-identical to the incoming ones (compare SHA-256) and report them as already archived. Report any other conflict, where the same path holds different content, and never overwrite it; the user decides. Only create a new folder when no existing one matches, and say so in the plan. If more than one folder could match, list them and ask.

Before transferring into an event destination, remind the user that it may be public or shared with external collaborators, and get explicit confirmation that everything being transferred, raw footage included, is appropriate for them to see.

## Workflow

### 1. Confirm scope

Confirm all of the following:

- The local project root.
- The destination: the DevEd folder by default, or an event destination only if the user says so.
- The video name and date for the folder name.
- The mode. **Move** is the default: transfer, verify, then remove the listed originals. **Copy** keeps the local originals; the user can always choose it.

Then check how the source and destination relate. Decide whether the local project root is itself inside a Drive-synced folder, and whether it is already inside the destination tree. If it is, its files are Drive-backed copies. Removing them would delete cloud data, so do not remove them. Report this, and treat the files as copy-only unless the user gives a separate, explicit instruction for those exact paths.

### 2. Inventory

List every file and folder under the project root, with its relative path, size in bytes, and modification time. Treat app bundles (such as `.screenstudio`), ZIP archives, and camera-card trees as single units, and record both their total sizes and their file counts. Look at the destination too: whether a matching project folder already exists there (see [Destinations](#destinations)) and what it contains.

Read only. Do not open projects in their editing apps, because that can auto-save or relink media.

### 3. Propose the mapping

Fill in [assets/archive-manifest-template.md](assets/archive-manifest-template.md):

- **Deliverables:** for each of the deliverables (archive, youtube, social, captions, thumbnail), the candidate source file, its size, its archive name, and your confidence. Mark each one as **missing** or **ambiguous** where that applies. For example, when two `.mov` files could be the archive master, list both and ask. Report every missing deliverable in the plan.
- **Source:** the critical source and supporting files you propose to keep, each mapped to a path under `source/`, with a line on which footage you judged critical.
- **Not archived (drop list):** timelines, intermediate copies, backups, duplicate media (confirmed with `cmp`), and temp files, each with a reason, following [references/what-to-keep.md](references/what-to-keep.md).
- **Transcodes:** every uncompressed or huge-intermediate clip to be transcoded to HEVC, with current and estimated size. Transcode by default, for all footage.
- **Slimmed size:** total to transfer against the original size, and the free space at the destination and on the local disk.
- **Existing folder:** whether you are archiving into an existing project folder, with its current contents listed. Files that are byte-identical (SHA-256) are skipped and reported as already archived.
- **Collisions:** any destination path that already exists with different content, or that two sources would share. Never overwrite. Propose keeping both or skipping, and let the user decide.
- **Totals:** file count and bytes to transfer, compared with the free space at the destination if that is visible.
- **Removal list (move mode):** the exact original paths that will be removed after verification. This is the transferred items plus the dropped items, minus anything inside a Drive-synced tree or the archive. Dropped items go to the Trash with the rest, so no copy stays behind.

### 4. Get approval

The plan is read-only. Present the manifest and the open questions, and revise it until the user explicitly approves it. In move mode, approving the manifest authorizes removing the listed originals after verification. No second approval is needed for those exact paths. Anything not on the removal list needs its own explicit instruction.

### 5. Transfer

Transfer exactly what the approved manifest lists, and nothing else.

- Treat a move into a Drive-mounted folder or onto another volume as three steps: copy, verify, then remove. Do this even if the operating system offers a one-step move.
- Use a copy method that keeps bundles, file metadata, and relative paths intact, and that refuses to overwrite.
- Rename only the archive copies of deliverables, never the source files.
- If something fails, or the destination has changed since the inventory, stop, remove nothing, and report.

### 6. Verify the transfer

Follow [references/verification.md](references/verification.md). For every transferred item, record:

- **Byte count:** the destination copy matches the source, and for folders and bundles, the file count matches too.
- **Checksum (SHA-256):** the destination contents, read back, match the source. This is required before removing an original. Seeing a file in a Drive folder, or an online-only placeholder, does not prove the transfer is complete.

If any item fails or cannot be checked, the transfer is **not verified**. Remove nothing, and report which items failed.

### 7. Remove the originals (move mode)

Only when every item is verified:

- Remove only the exact paths on the approved removal list. Never remove anything inside the archive, inside a Drive-synced tree, or outside the list.
- Prefer recoverable removal: the Mac Trash for local originals. If only permanent removal is available, say so before acting. Trashed files still use disk space until the trash is emptied.
- Move the original to the Mac Trash only after verification, so no copy remains in the original location. Leave emptied folders and the project root in place unless the user asks otherwise.
- Check free disk space before the next batch. The Trash holds space until it is emptied, so ask the user to empty it when space is tight.
- If a removal fails partway, stop and report exactly what was and wasn't removed.

### 8. Report

Report the local result and the cloud state as separate lines:

- **Archived locally:** the transfer is verified, originals are removed or kept, and any skipped or left-in-place items are listed.
- **Cloud status:** **sync pending / unverified** unless there is server-side evidence (see the verification reference), in which case it is **cloud backed up**. Never report "backed up" from local evidence alone.

When sync is pending and originals have been removed, the archive exists only on this machine until upload completes. Tell the user, and suggest they not disconnect the Drive account or clear its local cache until the upload finishes.

## Checking existing archives

When asked to check an archive the user made earlier, classify what is there against the target layout and report how it differs (see the naming reference). Do not rename or reorganize older archives unless the user asks for that specific change.
