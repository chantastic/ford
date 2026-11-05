# Media and application signals

Use these signals to classify files during inventory. They are hints, not proof. Application versions and user settings change where files live, so confirm with the user when the evidence is thin.

## Editing and compositing applications

| Application | Project signals | Application-managed or regenerable | Linking notes |
| --- | --- | --- | --- |
| DaVinci Resolve | Projects usually live in a Resolve project library (a database), not the project folder. Exports appear as `.drp` (project) or `.dra` (archive folder); timelines as `.drt`. | `CacheClip`, `OptimizedMedia`, `ProxyMedia`, render cache, gallery stills | Media is referenced by path. Moving media needs relinking in Resolve. A missing `.drp` in the folder does not mean there is no project. |
| Adobe Premiere Pro | `.prproj`; `Adobe Premiere Pro Auto-Save/`; Productions use a folder of `.prproj` files | Media cache (`.cfa`, `.pek`, often in a global location), proxies folder, preview renders | Media paths are stored in the project. Productions also depend on the folder structure itself. |
| Final Cut Pro | `.fcpbundle` library (a package directory); `.fcpxml` / `.fcpxmld` exports | Inside the library: render files, transcoded/proxy media, analysis files | Treat the library as one opaque unit. Media can be inside the library or external and referenced. Moving external media breaks references. |
| Adobe After Effects | `.aep`, `.aepx`; `Adobe After Effects Auto-Save/` | Disk cache (usually global), render queue output | Footage is referenced by path. Dynamic Link ties `.aep` files to Premiere projects. |
| Avid Media Composer | `.avp` project, `.avb` bins | `Avid MediaFiles/MXF/<n>/` managed media | Avid manages MXF media by folder. Never rename or reorganize inside `Avid MediaFiles`. |

## Screen recording and tutorial tools

Common in developer-education work.

| Tool | Signals | Notes |
| --- | --- | --- |
| OBS Studio | `.mkv`, `.mp4`, `.mov` recordings, often named by timestamp | Originals. Remuxed copies (`.mkv` to `.mp4`) can sit beside the source; ask before treating either one as a duplicate. |
| ScreenFlow | `.screenflow` package | Treat as one opaque unit, like a project file that contains media. |
| Camtasia | `.tscproj`, `.cmproj` (package), `.trec` recordings | Project files reference recordings and assets. |
| Screen Studio | `.screenstudio` project | Treat as opaque. |

## Camera-card structures

Preserve these trees exactly. Clips can span several files, and editing applications and offload tools rely on the folder layout and sidecar metadata.

| Pattern | Typical source |
| --- | --- |
| `DCIM/<nnn>XXXXX/` | Most stills and consumer cameras, action cameras (chaptered files such as `GX01nnnn.MP4`, `GX02nnnn.MP4`) |
| `PRIVATE/M4ROOT/CLIP/` | Sony XAVC cameras |
| `PRIVATE/AVCHD/BDMV/STREAM/*.MTS` | AVCHD cameras (spanned clips) |
| `CONTENTS/` | Canon cinema cameras |
| `*.RDM/*.RDC/*.R3D` | RED (one clip split across numbered segments) |
| `*.braw` with sidecars | Blackmagic RAW |
| `.mhl`, `.md5`, `.xxh*` files | Checksums from offload tools. Keep them with the card they describe. |

## Audio

- Field and recorder files: `.wav` (often from multitrack recorders with numbered takes), `.bwf`.
- Sessions: `.sesx` (Audition), `.logicx` package (Logic Pro), `.ptx` with `Audio Files/` (Pro Tools). Session folders depend on their relative layout.
- Deliverables: mixes, stems, and music licensed for the project. Keep license documents with the music they cover.

## Graphics

- Source design files: `.psd`, `.ai`, `.afphoto`, `.afdesign`, `.sketch`, `.mogrt` (motion-graphics templates).
- Rendered assets: `.png`, `.jpg`, `.svg`, `.mov` with alpha (for example, ProRes 4444).

## Exports and deliverables

- Masters and mezzanine files: ProRes `.mov`, DNx `.mxf` / `.mov`.
- Platform deliverables: `.mp4` (H.264 or HEVC), thumbnails, caption files (`.srt`, `.vtt`), chapter lists.
- Review cuts are often labeled `draft`, `v<n>`, `review`, or with dates. Ask which version is final before you classify anything as superseded.

## Caches and system files

- Application caches and renders, as listed in the tables above.
- Waveform and peak files: `.pek`, `.pkf`, `.cfa`.
- macOS metadata: `.DS_Store`; `._*` AppleDouble files on non-Mac-formatted drives. AppleDouble files can carry real metadata, so flag them and do not dismiss them as junk.
- Thumbnails and preview folders created by asset managers.

Regenerable does not mean safe to delete. Rebuilding caches and proxies can take hours, so list them as candidates only.
