# Archive manifest: yyyy-mm-dd-{video-name}

> Proposed. Nothing has been transferred or removed until the approval below is checked.

- **Local project root:** `{local-project-root}`
- **Project type:** educational / event
- **Mode:** move (copy, verify, remove listed originals) / copy (keep local originals)
- **Destination:** `{archive-root:<type>}/yyyy-mm-dd-{video-name}/`
- **Source inside a Drive-synced tree:** yes / no (if yes, the source files are cloud-backed and are never removed; copy only)
- **Destination already exists:** yes / no
- **Shared with external collaborators:** yes / no / unknown (event destinations: confirm the contents are appropriate)

## Deliverables (archive root)

| Deliverable | Source (relative) | Bytes | Archive name | Status | Notes |
| --- | --- | --- | --- | --- | --- |
| Master | `exports/example-final.mov` | 12,345,678,901 | `yyyy-mm-dd-{video-name}-youtube.mov` | found / ambiguous / missing | |
| X transcode | | | `yyyy-mm-dd-{video-name}-x.mp4` | missing | Not produced; will not be generated |
| Thumbnail | `assets/thumb.png` | 1,234,567 | `yyyy-mm-dd-{video-name}-thumbnail.png` | ambiguous | Two candidate images; confirm |
| Captions | `example.itt` | 45,678 | `yyyy-mm-dd-{video-name}-transcript.itt` | found | ITT kept as-is |

## Source (`source/`)

| Source (relative) | Bytes | Archive path | Unit | Notes |
| --- | --- | --- | --- | --- |
| `screencapture/example-recording.zip` | 98,765,432,100 | `source/screencapture/example-recording.zip` | ZIP, not unpacked | Original recording, unchanged |
| `assets/` | 2,345,678,901 | `source/assets/` | folder | Music, SFX, images |

## Not archived (proposed)

| Path (relative) | Bytes | Reason | Confirmed by user |
| --- | --- | --- | --- |
| `cache/` | 4,567,890,123 | Regenerable render cache | [ ] |

## Collisions

| Archive path | Conflict | Proposed resolution |
| --- | --- | --- |
| | | keep both / skip (never overwrite) |

## Totals

- Files to transfer: N
- Bytes to transfer: N
- Destination free space (if visible): N

## Linked media

- {project file} references media by path; the new `source/` layout changes / preserves those paths.

## Transfer verification (gates removal)

| Item | Copy completed | Bytes match | Checksum match | Status |
| --- | --- | --- | --- | --- |
| `yyyy-mm-dd-{video-name}-youtube.mov` | yes / no | yes / no | yes / no (SHA-256 or MD5) | verified / failed / not checked |
| `source/screencapture/example-recording.zip` | yes / no | yes / no | yes / no | verified / failed / not checked |

Overall transfer: **verified / not verified**. If not verified, nothing is removed.

## Removal list (move mode)

Exactly the originals covered by this request. Approving this manifest authorizes their removal after the transfer is verified. Nothing else is removed.

| Local path (relative to project root) | Bytes | Inside synced tree or archive | Removal method | Result |
| --- | --- | --- | --- | --- |
| `exports/example-final.mov` | 12,345,678,901 | no (yes means excluded and kept) | OS trash / permanent (stated before acting) | removed / kept / failed |
| `screencapture/example-recording.zip` | 98,765,432,100 | no | OS trash | removed / kept / failed |

Items under "Not archived" are left in place and are not on this list.

## Report

- **Archived locally:** verified / not verified; originals removed / kept
- **Cloud status:** cloud backed up (evidence: connector size + MD5 / user-confirmed) / sync pending / unverified
- If sync is pending and originals were removed, the archive exists only on this machine until upload completes.

## Approval

- [ ] Manifest approved in **move** mode: transfer, verify, then remove the listed originals
- [ ] Or: manifest approved in **copy** mode: keep local originals
