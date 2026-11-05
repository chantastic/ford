# Verification

Archive status has two separate claims. Record evidence for each one, and never let one stand in for the other.

| Claim | What it means | Status values | Gates |
| --- | --- | --- | --- |
| Transfer verified | The destination contains every approved item, and its contents match the source | `verified`, `failed`, `not checked` | Removing local originals (move mode) |
| Cloud backed up | Google Drive's servers hold the files | `cloud backed up`, `sync pending / unverified`, `failed` | Nothing. It is reported, not required. |

In move mode, the approved manifest authorizes removing its listed originals once the transfer is verified. Cloud status does not block that removal, but it must be reported honestly.

## Transfer verification

Required before removing any original:

1. **Copy completed:** the copy operation reported success for every item, with no skipped or partial files.
2. **Byte count:** each destination file's size equals the source's. For a folder, bundle, or camera-card tree, the file count and total bytes also match.
3. **Checksum:** a checksum (for example SHA-256 or MD5) of the destination contents, read back from disk, matches the source. For folders and bundles, compare per file.

Insufficient on their own:

- A file name appearing in a mounted Drive folder.
- An online-only placeholder, whose reported size comes from the cloud rather than from local content.
- A size match without reading back the contents.

Newly copied files in a mounted Drive folder are normally stored locally while their upload is pending, so reading them back does not need a download. If a destination file turns out to be online-only, reading it can trigger a download. Tell the user, and don't treat the placeholder itself as evidence.

Hashing very large recordings takes time. If the user doesn't want to wait, keep the originals (copy mode) instead of removing them without a checksum.

## Cloud status

A file in a mounted Drive folder proves only that the local sync client has it. Its upload may still be pending, paused, or failed. Local checksums show nothing about the cloud.

Evidence for **cloud backed up**, depending on what is available:

- **Drive connector or API, if authorized:** the file exists at the expected location, and its reported size matches. Where the service reports a content checksum (Drive reports MD5 for regular binary files), compare it with the local MD5. Bundles and folders have no single checksum, so check their contents file by file, or by count and total size.
- **Sync client status shown to the user:** the user confirms that the desktop sync client shows no pending or failed uploads for the destination. Record this as user-confirmed.
- **Drive web interface:** the user confirms that the files appear on drive.google.com with the expected sizes. Record this as user-confirmed.

Do not read sync-client databases, logs, tokens, or browser sessions to infer status. Without evidence, report **sync pending / unverified**.

## Synced-tree checks

Before removing anything, check every path on the removal list:

- **Inside the archive destination:** never remove it.
- **Inside any Drive-synced folder:** never remove it as part of a move. Removal propagates to the cloud. Report it and keep it.
- **Outside synced folders (a plain local working copy) and on the approved removal list:** remove it once the transfer is verified.

Never use a naive "move, then delete" on synced trees. A move must never cause a deletion inside the archive or in the cloud.

## Removal

- Remove only the exact approved paths, and only when every item is transfer-verified. If any item fails, remove nothing.
- Prefer recoverable removal, such as the operating system's trash, only if the host actually supports it. Do not promise a recovery option that you can't confirm exists. If only permanent removal is available, say so before acting.
- Copy mode, which keeps the local originals, is always an acceptable outcome.
