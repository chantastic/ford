# Dry-run manifest: <project label>

> Dry run. No files have been moved, renamed, copied, or deleted.

- **Authorized root:** `<root path as given by the user>`
- **Inventory date:** <YYYY-MM-DD>
- **Editing application(s):** <application and version, if known> (confidence: high / medium / low)
- **Project state:** archive / in progress / setup

## Inventory summary

| Folder (relative) | Files | Size | Main types | Bucket | Notes |
| --- | --- | --- | --- | --- | --- |
| `example/footage/A001/` | 120 | 210 GB | `.mp4`, `.xml` | Originals | Camera-card tree, keep intact |

## Proposed layout

```text
<proposed tree, relative to the root>
```

Rationale: <why this layout fits this project's inventory>

## Operations

| # | Operation | Source (relative) | Destination (relative) | Reason | Re-link needed |
| --- | --- | --- | --- | --- | --- |
| 1 | move | `footage/A001/` | `02-originals/A001/` | Group camera originals | Yes, in <application> |

## Collisions

| Destination | Conflict | Proposed resolution |
| --- | --- | --- |
| `06-exports/final.mp4` | Already exists | Keep both; rename incoming to `final-<date>.mp4` (needs approval) |

## Left in place

| Path (relative) | Reason |
| --- | --- |
| `Example.fcpbundle` | Application-managed library; treated as opaque |

## Deletion candidates (separate approval required)

Not part of the operations above. Each item needs its own explicit approval.

| Path (relative) | Size | Why it is regenerable | Cost to regenerate |
| --- | --- | --- | --- |
| `CacheClip/` | 40 GB | Resolve render cache | Re-render on next playback |

## Rollback strategy

1. Before any execution: confirm a verified backup or snapshot of the root.
2. Reverse map: undo each operation by moving its destination back to its source, in reverse order (last operation first).
3. Verification: compare file counts and total size before and after; compare checksums for originals.
4. Re-link: if relinking was performed, reopen the project and confirm no media is offline.

## Open questions

- <question for the user>

## Approval

- [ ] Layout approved
- [ ] Operations approved (execution is a separate request)
- [ ] Deletion candidates reviewed (each needs separate approval)
