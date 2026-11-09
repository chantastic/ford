---
name: sponsored-playlist-sync
description: Keep a sponsor company's "Sponsored by" YouTube playlist complete. Use when the user wants to check, audit, or update the playlist of sponsored creator videos, or asks which live sponsored placements are missing from it. Collects live YouTube placements from the sponsor's partner Slack channels, excludes review cuts, non-YouTube and not-yet-live placements, verifies each candidate with yt-dlp (public, creator's main channel, full episode, sponsor mentioned), diffs against the playlist, adds missing videos with a playlist-add tool if one is connected, re-verifies, and reports what is missing, excluded, or duplicated.
---

# Sponsored playlist sync

> Draft skill. Review it before relying on it.

Make sure every live, public sponsored video that features the sponsor is on the sponsor's "Sponsored by" playlist. The work is mostly evidence gathering: find what went live, prove each video qualifies, compare with the playlist, then add only what is missing.

## Ground rules

- **Configuration, not constants.** The sponsor name, the playlist URL, the partner Slack channels (and their former names), and each creator's main YouTube channel come from your own memory or the user's configuration for this run. If any is missing, ask. Do not guess, and do not write them into skills, repos, or other shared files.
- **Read Slack, don't post.** Search and read the partner channels. Do not reply, react, or message anyone.
- **Live and public only.** A link in Slack is a lead, not proof. Only a video that `yt-dlp` confirms as public on the creator's main channel qualifies.
- **Adding is the only write.** Never remove, reorder, or edit playlist entries, and never change a video's metadata. Report duplicates and stale entries instead of fixing them.
- **No credential shortcuts.** Write to the playlist only through a connected tool the user has authorized. Never read browser sessions, cookies, or tokens to reach YouTube Studio.

## Configuration

| Parameter | What it is |
| --- | --- |
| `{sponsor}` | The sponsor company name, plus any product names that appear in descriptions |
| `{sponsor-playlist}` | The public URL of the "Sponsored by" playlist |
| `{partner-channels}` | Slack channels shared with creators and agencies, with any former names |
| `{creator-channels}` | Each creator's main YouTube channel (ID or handle) |

## Workflow

### 1. Collect candidates

Search `{partner-channels}` by current and former names, because channels get renamed and old threads keep the old context. Read whole threads, not just top-level messages: the live link often appears as a reply.

Take YouTube links posted as live placements: "is live", "went live", "placement", "it's up", and similar. Note the creator, the message date, and the thread, so each candidate can be traced back.

### 2. Exclude what isn't a live placement

Set aside, with a reason for each:

- **Review or approval cuts:** Lawn, Frame.io, Drive previews, and private, unlisted-for-review, or 404 YouTube links shared for sign-off.
- **Non-YouTube placements:** newsletters, X posts, podcasts without a YouTube video, and similar.
- **Pitch examples:** past videos shared to show a creator's style or reach.
- **Not yet live:** scheduled or upcoming placements. Mention these so the user can check back.

If a thread is ambiguous, for example a draft link that later went public at the same URL, keep it as a candidate and let verification decide.

### 3. Verify each candidate

```bash
yt-dlp -j --skip-download "<video url>"
```

From the JSON, check:

- `availability` is `public`. A failed fetch, or `private`, `unlisted`, or `needs_auth`, excludes the video.
- `channel_id` or `uploader_id` matches the creator's main channel in `{creator-channels}`. A clips or second channel is an exclusion unless the user says otherwise.
- `duration` looks like a full episode. Flag short videos that look like a standalone ad read, and ask rather than deciding.
- `description` mentions `{sponsor}` or its products. If it doesn't, flag the video instead of dropping it silently; the sponsorship may be spoken only.

Keep `id`, `title`, `webpage_url`, and `upload_date` for the report.

### 4. Diff against the playlist

```bash
yt-dlp --flat-playlist --print id "{sponsor-playlist}"
```

Compare by video ID, not URL, since links come in several forms (`youtu.be`, `watch?v=`, `shorts/`, with timestamps). Note any ID that appears more than once on the playlist, and any entry that is no longer public.

### 5. Add the missing videos

If the playlist-add tool is connected (the video app's MCP tool or endpoint that adds videos to a playlist idempotently), add the verified missing IDs with it. Add only those IDs.

If no write tool is available, give the user a clean list of the missing links to paste into YouTube Studio, and stop there.

### 6. Re-verify

Run the playlist diff again. Confirm each added video now appears exactly once, and recount.

### 7. Report

- **Playlist count:** before and after.
- **Added:** `[title](url), creator` for each.
- **Still missing:** `[title](url), creator` for each, with why (no write tool, add failed, awaiting a decision).
- **Excluded:** each set-aside link with its creator and reason, including flagged ad-read lengths and descriptions without a sponsor mention.
- **Duplicates and stale entries:** IDs on the playlist more than once, or no longer public. Reported only, not changed.
- **Upcoming:** placements that aren't live yet.
