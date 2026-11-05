# Naming and layout

The names below are generic patterns. Real projects, especially older ones, will differ. Classify what you find; don't assume it already matches.

## Deliverables (archive root)

| Deliverable | Target name | Accept as a candidate | Do not assume |
| --- | --- | --- | --- |
| Full-quality master | `yyyy-mm-dd-{video-name}-youtube.mov` | `.mov` or other high-bitrate render of the final cut | That the biggest or newest `.mov` is the final cut |
| X transcode | `yyyy-mm-dd-{video-name}-x.mp4` | `.mp4` render sized for X | That any `.mp4` is the X version; it could be a draft or a review cut |
| Thumbnail | `yyyy-mm-dd-{video-name}-thumbnail.jpg` or `.png` | Still image used as the published thumbnail | That any image in the project is the thumbnail |
| Captions | `yyyy-mm-dd-{video-name}-transcript.srt` | `.srt`; `.itt` (iTunes Timed Text) is a valid variant. Keep its extension; don't convert it. | That a text file is the final transcript |

If a deliverable is missing, list it as missing. Do not produce it unless the user asks for that as a separate task.

## Source (`src/`)

Everything that is not a deliverable but is needed to understand or rebuild the video:

- Original recordings: camera files, screen recordings, microphone tracks.
- Recorder project bundles and archives, such as a `.screenstudio` bundle or a Screen Studio ZIP export. Copy them as single units. Do not unzip or repackage them.
- Editing project files, such as `.drp`, `.prproj`, `.fcpbundle`, and `.aep`.
- Supporting assets: B-roll, sound effects, music (with any license files), images, and graphics.

Keep camera-card trees (such as `DCIM/…` or `PRIVATE/…`) intact, and keep checksum sidecars (`.mhl`, `.md5`) next to the files they describe. If an editing project references media by relative path, keep that relative arrangement inside `src/`. If the new layout would break references, say so in the manifest rather than quietly reshuffling files.

## Variants you will see in existing projects

Examples of layouts already in use. Any one project may mix several of these:

- Sources already in `src/`.
- Screen recordings in `screencapture/`, sometimes as a Screen Studio ZIP.
- Supporting files in an `assets/` folder next to the recordings, not inside `src/`.
- Deliverables inside an `exports/` subfolder rather than at the root.
- Undated or differently dated folder names, other suffixes (`-final`, `-v2`, `-yt`), and captions in `.itt` instead of `.srt`.

For a **new archive**, map these into the target layout in the archive copy, and record each mapping and its reason. For example, `screencapture/` maps to `src/screencapture/`.

For an **existing archive**, report the variants and leave them alone. Do not rename or restructure older archives unless the user asks for that specific change.

## Things that are usually not archived

Propose leaving these out, with a reason, and let the user confirm. Never treat them as safe to delete:

- Application caches and render caches (proxy, optimized media, waveform and peak files).
- Auto-save folders, if the main project file is archived.
- Operating system metadata (`.DS_Store`). AppleDouble `._*` files can carry real metadata, so ask about them.

Rendered files are not automatically cache. A render can be a deliverable or a graphics asset, so confirm before leaving one out.
