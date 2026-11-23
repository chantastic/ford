# Naming and layout

The names below are generic patterns. Real projects, especially older ones, will differ. Classify what you find; don't assume it already matches.

## Deliverables (archive root)

| Deliverable | Target name | Accept as a candidate | Do not assume |
| --- | --- | --- | --- |
| Archive master | `yyyy-mm-dd-{video-name}-archive.mov` | The full-quality master export of the final cut, labeled ` - archive` in the project. H.265 these days. | That the biggest or newest `.mov` is the final cut |
| YouTube upload | `yyyy-mm-dd-{video-name}-youtube.mp4` | The H.264 file uploaded to YouTube, labeled ` - youtube`, or ` - upload` on older projects. H.264 because YouTube is reported to handle it better. | That any `.mp4` is the upload; it could be a draft or a review cut |
| Social cut | `yyyy-mm-dd-{video-name}-social.mp4` | H.264 at 1080p, following the social platforms' bitrate guidance, labeled ` - social` | That a vertical short or teaser is the social cut |
| Captions | `yyyy-mm-dd-{video-name}-captions.srt` | `.srt`. An `.itt` (iTunes Timed Text) or other format found on older projects keeps its extension; don't convert it. | That a text file is the final transcript |
| Thumbnail | `yyyy-mm-dd-{video-name}-thumbnail.jpg` or `.png` | Still image used as the published thumbnail | That any image in the project is the thumbnail |

### Older projects and `- upload`

On older projects (2026 and earlier), the labels differ. A file labeled ` - upload` is ambiguous: it could be the archive master or the YouTube upload. If it is the only master-class export, it is an acceptable archive master and also serves as the YouTube upload. Archive it once and name it for both roles in the manifest, rather than inventing a second file. If both ` - upload` and a ` - archive` exist, treat ` - archive` as the archive master and ` - upload` as the YouTube upload. Confirm with the user when it is unclear.

If a deliverable is missing, report it as missing in the plan. Do not produce it unless the user asks for that as a separate task.

## Source (`source/`)

Everything that is not a deliverable and is worth keeping goes in `source/`, in one bucket with no sub-taxonomy (see [what-to-keep.md](what-to-keep.md) for the full keep, drop, and compress rules):

- Critical source: interviews, a-roll, and similar primary footage, including raw b-roll and screen recordings. Uncompressed footage is transcoded to HEVC; compressed footage is kept as is.
- Recorder project bundles and archives, such as a `.screenstudio` bundle or a Screen Studio ZIP export. Copy them as single units. Do not unzip or repackage them.
- Small planning, transcript, publish, and script files.
- Supporting assets: B-roll, sound effects, music (with any license files), logos, images, and graphics.

Do not archive timelines: NLE projects and libraries (`.fcpbundle`, `.prproj`, `.drp`, `.aep`) and FCPXML or EDL files.

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

Timelines, intermediate copies (review cuts, render files and caches), backup and auto-save libraries, byte-identical duplicate media, and lock, temp, and `.DS_Store` files. See [what-to-keep.md](what-to-keep.md). List each with a reason in the manifest and let the user confirm.

Rendered files are not automatically droppable. A render can be a deliverable (a finished vertical short or teaser) or a graphics asset, so confirm before leaving one out.
