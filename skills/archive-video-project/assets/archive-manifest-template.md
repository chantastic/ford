# Archive manifest: yyyy-mm-dd-{video-name}

> Proposed. Nothing has been copied or removed until the approvals below are checked.

- **Local project root:** `{local-project-root}`
- **Project type:** educational / event
- **Destination:** `{archive-root:<type>}/yyyy-mm-dd-{video-name}/`
- **Source inside a Drive-synced tree:** yes / no (if yes, the source is a cloud-backed copy and is excluded from cleanup)
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

- Files to copy: N
- Bytes to copy: N
- Destination free space (if visible): N

## Linked media

- {project file} references media by path; the new `source/` layout changes / preserves those paths.

## Verification

| Item | Copy check | Copy status | Remote evidence | Remote status |
| --- | --- | --- | --- | --- |
| `yyyy-mm-dd-{video-name}-youtube.mov` | bytes / SHA-256 | verified / failed / not checked | connector size + MD5 / user-confirmed / none | verified / unverified / failed |

Overall: **verified / unverified**. If unverified, cleanup is blocked.

## Local cleanup (optional; only after full verification)

| Local path (exact) | Bytes | Inside synced tree | Removal method | Approved |
| --- | --- | --- | --- | --- |
| | | no (yes means excluded) | OS trash / permanent | [ ] |

## Approvals

- [ ] Mapping approved; the copy may proceed
- [ ] Each cleanup path approved individually (approving the policy is not enough)
- [ ] Or: keep local originals
