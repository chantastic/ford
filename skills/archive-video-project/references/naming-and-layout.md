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

## Source (`source/`)

The important stuff that is not a deliverable (see [what-to-keep.md](what-to-keep.md) for the full keep and drop rules):

- Raw footage: camera files, screen recordings, microphone tracks. The user's own huge ProRes a-roll is transcoded to HEVC; see what-to-keep.md.
- Recorder project bundles and archives, such as a `.screenstudio` bundle or a Screen Studio ZIP export. Copy them as single units. Do not unzip or repackage them.
- Small planning, transcript, publish, and script files.
- Supporting assets: B-roll, sound effects, music (with any license files), logos, images, and graphics.

Do not archive editing-app libraries or project files (`.fcpbundle`, `.prproj`, `.drp`, `.aep`) unless the user asks for a specific one.

Keep camera-card trees (such as `DCIM/…` or `PRIVATE/…`) intact, and keep checksum sidecars (`.mhl`, `.md5`) next to the files they describe.

## Variants you will see in existing projects

Examples of layouts already in use. Any one project may mix several of these:

- Sources in an older `src/` folder, or already in `source/`.
- Screen recordings in `screencapture/`, sometimes as a Screen Studio ZIP.
- Supporting files in an `assets/` folder next to the recordings, not inside the source folder.
- Deliverables inside an `exports/` subfolder rather than at the root.
- Undated or differently dated folder names, other suffixes (`-final`, `-v2`, `-yt`), and captions in `.itt` instead of `.srt`.

For a **new archive**, map these into the target layout in the archive copy, and record each mapping and its reason. For example, `screencapture/` maps to `source/screencapture/`, and the contents of an older `src/` map to `source/`.

For an **existing archive**, report the variants and leave them alone. Do not rename or restructure older archives unless the user asks for that specific change.

## Things that are not archived

Intermediate and review cuts, render files and caches, backup and auto-save libraries, editing-app libraries, byte-identical duplicate media, and lock, temp, and `.DS_Store` files. See [what-to-keep.md](what-to-keep.md). List each with a reason in the manifest and let the user confirm.

Rendered files are not automatically droppable. A render can be a deliverable (a finished vertical short or teaser) or a graphics asset, so confirm before leaving one out.
