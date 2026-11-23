# What to keep, drop, and compress

An archive is the five root deliverables plus one `source/` bucket of critical source. Apply these rules when filling in the manifest, and show the result as keep, drop, and transcode lists with sizes.

## Keep

- **Deliverables at the root:** `archive` (master export, H.265; on older projects an `upload` H.264 can serve), `youtube` (H.264), `social` (H.264, 1080p), `captions` (.srt), and `thumbnail`. See [naming-and-layout.md](naming-and-layout.md). Report any that are missing in the plan. If the video is published, pull the thumbnail from the published video rather than using a draft frame. If it is not published, use the best candidate and say so.
- **Everything else goes in `source/`, in one bucket.** No sub-taxonomy is needed. It holds:
  - **Critical source:** interviews, a-roll, and similar primary footage. Raw b-roll and screen recordings count too. Use judgment, and list what you chose in the plan.
  - **Small supporting files:** scripts, outlines, transcripts, publish notes, music (with any license files), logos, and graphics.

## Drop

Drop these, list each one with a reason, and have the user confirm the list as part of the manifest:

- **Timelines:** NLE projects and libraries (Final Cut `.fcpbundle`, Premiere `.prproj`, Resolve `.drp`, After Effects `.aep`), and timeline exchange files (FCPXML, EDL). The user does not need them, so there is nothing to relink.
- **Intermediate copies:** `v1` and other review or draft cuts, `edit/` working folders, render files, and caches (proxy, optimized media, waveform and peak files).
- **Backup and auto-save libraries.**
- **Duplicate media:** byte-identical copies, such as the Original Media an editor makes inside its library. Confirm they are identical with `cmp` first. A file that merely looks similar is not a duplicate.
- **Lock, temp, and `.DS_Store` files.** AppleDouble `._*` files can carry real metadata, so ask first.

A render is not automatically droppable. A finished vertical short or teaser may be the `social` deliverable. If you cannot tell, ask.

## Always compress critical source

This applies to all footage, whoever shot it.

- **Already compressed** (HEVC, H.264, and similar, including most camera files and screen recordings): keep as is.
- **Not compressed** (ProRes, uncompressed, or other huge intermediates): transcode to HEVC and drop the original once the transcode is checked. On macOS, use `hevc_videotoolbox` at about 15 Mbps, at the source resolution and frame rate, with AAC audio at 256k. Check the result: the duration matches the source, it plays, and the audio is in sync.

Transcode by default. List every transcode in the plan with its current and estimated size.

## Plan first, read-only

Before moving anything, show the user the plan: destination, the five deliverables (and any missing), the keep, drop, and transcode lists, the slimmed size against the original size, and free disk space. Wait for approval. The plan is read-only; do not open projects in their editing apps.

Check free disk space between batches when archiving several projects. A Trash that has not been emptied still holds disk space, so ask the user to empty it when space is tight.
