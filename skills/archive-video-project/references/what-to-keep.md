# What to keep, drop, and transcode

An archive holds the important stuff only. Apply these rules when filling in the manifest, and show the result as keep and drop lists with sizes.

## Keep

- **Deliverables at the root:** the master, the X and vertical cuts, captions or transcript, and the thumbnail. If the video is published, pull the thumbnail from the published video rather than using a draft frame. If it is not published, use the best candidate and say so.
- **Raw footage in `source/`:** camera files, screen recordings, and microphone tracks.
- **Small planning files in `source/`:** scripts, outlines, transcripts, publish notes, and cut lists.
- **Needed assets in `source/`:** music (with any license files), logos, and graphics.

## Drop

Drop these by default, list each one with a reason, and have the user confirm the list as part of the manifest:

- **Intermediate and review cuts:** `v1` and other draft renders, and `edit/` working folders.
- **Render files and caches:** proxy, optimized media, waveform and peak files, and render folders.
- **Backup and auto-save libraries.**
- **Duplicate media:** byte-identical copies, such as the Original Media a video editor makes inside its library. Confirm they are identical with `cmp` before calling them duplicates. A file that merely looks similar is not a duplicate.
- **Lock, temp, and `.DS_Store` files.** AppleDouble `._*` files can carry real metadata, so ask first.

A render is not automatically droppable. If it is a finished vertical short or teaser, it is a deliverable. If you cannot tell, ask.

## No editing-app libraries

Do not archive NLE libraries or project files (Final Cut `.fcpbundle`, Premiere `.prproj`, Resolve `.drp`, After Effects `.aep`) unless the user asks for a specific one. The user does not need them, so there is nothing to relink. Raw footage and deliverables are enough to rebuild from.

## Transcode

Original source normally goes in untouched. These are the exceptions.

- **The user's own on-camera a-roll** that exists as ProRes or another huge intermediate: transcode it to HEVC and drop the original once the transcode is checked. On macOS, use `hevc_videotoolbox` at about 15 Mbps, at the source resolution and frame rate, with AAC audio at 256k. Check the result: duration matches the source, it plays, and the audio is in sync.
- **Anyone else's footage, or event camera footage:** ask before transcoding. Do not assume.
- **Already-compressed footage** (HEVC or H.264 camera files, screen recordings): archive as-is.

List transcode candidates in the manifest with the current size and the estimated size. The user approves them like any other line.

## Plan first, read-only

Before moving anything, show the user the plan: destination, deliverables, keep and drop lists, transcode candidates, the slimmed size against the original size, and free disk space. Wait for approval. The plan is read-only; do not open projects in their editing apps.

Check free disk space between batches when archiving several projects. A Trash that has not been emptied still holds disk space, so ask the user to empty it when space is tight.
