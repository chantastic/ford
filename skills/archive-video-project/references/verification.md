# Verification

Archive verification has two separate claims. Record evidence for each one, and never let the first stand in for the second.

| Claim | What it means | Status values |
| --- | --- | --- |
| Copy verified | The destination contains every approved item, intact | `verified`, `failed`, `not checked` |
| Remote persistence verified | Google Drive's servers hold the files | `verified`, `unverified`, `failed` |

Cleanup of local originals requires **both** claims to be `verified` for every item being removed.

## Copy verification

Achievable evidence, from strongest to weakest:

1. Checksums (for example SHA-256 or MD5) of source and destination files, when both can be read locally. This is slow for large recordings, so ask before hashing very large files.
2. Exact byte count for every file, plus the total file count.
3. Total bytes and file count per folder, which is acceptable only for app bundles and card trees treated as units.

On a streamed or online-only mounted Drive, reading a destination file for hashing can trigger a download. Tell the user before you do it.

## Remote persistence verification

A file appearing in a mounted Drive folder proves only that the local sync client knows about it. The upload may still be pending, paused, or failed. Local checksums of the mounted copy don't prove anything about the cloud.

Acceptable evidence, depending on what is available:

- **Drive connector or API, if authorized:** the file exists at the expected location, and its reported size matches the local byte count. Where the service reports a content checksum (Drive reports MD5 for regular binary files), compare it with a local MD5 of the source. A match is strong evidence. Bundles and folders don't have a single checksum, so verify their contents file by file or by count and total size.
- **Sync client status shown to the user:** the user confirms that the desktop sync client shows no pending or failed uploads for the destination. This is weaker evidence; record it as user-confirmed.
- **Drive web interface:** the user confirms that the files appear on drive.google.com at the expected location with the expected sizes. Record this as user-confirmed.

Do not read sync-client databases, logs, tokens, or browser sessions to infer status.

If none of these is possible, record **unverified**, explain what would be needed, and stop before any cleanup. It is fine to finish the run with an unverified archive; the user can verify later.

## Synced-tree checks

Before proposing any removal, check every path:

- **Inside the archive destination:** never remove it.
- **Inside any Drive-synced folder:** removing it removes the cloud copy too, and that change propagates. Exclude it from cleanup unless the user explicitly says otherwise for that exact path.
- **Outside synced folders (a plain local working copy):** eligible for cleanup only with explicit, per-path approval after both verifications.

Never do "move to archive, then delete the source." On a synced tree, that can propagate a deletion to the archive.

## Removal

If the user approves removing specific local originals:

- Prefer recoverable removal, such as the operating system trash, only if the host actually supports it. Do not promise a recovery option that you can't confirm exists.
- If the only available operation is permanent, say so before acting.
- Keeping local originals is always an acceptable outcome.
