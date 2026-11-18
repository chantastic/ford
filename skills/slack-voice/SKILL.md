---
name: slack-voice
description: The user's Slack voice, built from their real DMs and channel posts. Use when drafting, rewriting, or reviewing a Slack message, reply, thread reply, or announcement on the user's behalf. Loads the writing-voice skill first, then adds Slack-only rules - lowercase by default, no greetings or sign-offs, very short messages and fragments, registers by place (close DMs, group DMs, internal channels, partner-shared channels, leadership-visible threads, company-wide posts), recurring phrases, a drafting checklist, and before/after examples.
---

# Slack voice

> Draft skill. Review it before relying on it.

**Load `writing-voice` first** (in this plugin, `../writing-voice/SKILL.md`). It holds the base tone rules and the phrases to avoid. This skill adds only what's specific to Slack; where they differ, this skill wins for Slack.

Slack is looser than email and never signed. Don't carry email habits (greetings, sign-offs, composed paragraphs) into Slack. DMs and public channels are close cousins: default to the DM register in 1:1s and small groups, tighten slightly in public channels, and a bit more in partner-shared channels and company-wide posts.

## Shared habits

- **Lowercase by default.** Most DMs and most channel posts start lowercase. Capitalize after an `@mention` line or in a rare composed announcement.
- **No greetings or sign-offs.** Never `Hi [Name],` or `Thanks, [name]`. `hiya` or `hiya team` is the one exception, and it's the whole greeting.
- **Short, then shorter.** A typical message is a dozen words. A longer thought is often split into several messages in a thread rather than one tidy paragraph.
- **Fragments are complete messages.** `yup.` / `word.` / `for sure.` / `gooot it`.
- **Light punctuation.** Many messages end without a period. Questions get `?`. Exclamation marks are real but infrequent (`thank you!`, `love it!`). Occasional ellipses. Em dashes almost never.
- **Casual spellings:** `i'm`, `gonna`, `y'all`, `tho`, `thru`.
- **Sparing emoji:** an occasional slightly-smiling face, disappointed face, or thumbs-up. Never decorate every message.
- **Humor** is dry, a little typo-playful, often at their own expense: `hahah.`, `ha!`, `lol. i'm a dummy.`, `noice.`, `woord.`, `heeeeeeeck yes!`
- **Links** pasted directly, often with a short setup line: `video up, unlisted and embeddable.` plus the link.
- **Mentions are functional:** `@[Name]` then the point, or a `cc @[Name] @[Name]` line followed by what they'll do.

## Registers by place

| Place | Register |
| --- | --- |
| Close colleague DMs | Loosest. Fragments, `word.`, honesty, personal asides, occasional heat when the topic calls for it. |
| Group DMs | Same as DMs, with maybe one extra line of context so everyone can follow. |
| Internal work channels | Lowercase, short, practical. Status plus link. Blunt is ok; profanity usually isn't. |
| Partner-shared channels | `hiya team`, why they're writing, a specific question, mentions. A little more complete, still casual. |
| Leadership in the thread | Still casual and lowercase: `@[Name] yes. a video asset pack would be great.` No sudden formality. |
| Company-wide posts | A one-line hook, maybe a joke at their own expense, a link. Slightly more composed, never corporate. |

## DMs and group DMs

- **Greeting:** usually none. `hiya.` to open a new topic, and not even that if the DM is already active.
- **Length:** one to four lines. Longer only to answer a real question, then short paragraphs or a few bullets, still lowercase.
- **Yes:** `yeah` / `yeah!` / `yup.` / `for sure.` / `word, word.` / `yes!`, often with the next step in the same breath: `yeah! put something on the books for next week. we'll jam.`
- **No:** plain, sometimes with a reason: `i love that event but won't be able to drive a project that week. do i need to explicitly opt out?` / `sadly all recorded :disappointed:`
- **Thanks:** `thank you!` / `thank you! that's super helpful` / `awesome thanks!` / `thank you. thank you. thank you.` Never "I really appreciate your help."
- **Asks:** direct and lowercase: `when is our next launch week?` / `could you forward to me?` / `should we build our own next year?`
- **Pushback or venting:** close DMs are where they're most unfiltered. Say what's broken, then what they'll do instead. Only with trusted colleagues, and only when the topic calls for it. Never in a public or partner channel.
- **Coordinating:** thinks out loud and names the practical need: `hiya.` plus a short setup, a question (`do you see a need for...?`), and an honest constraint.
- **Honesty:** `i'll be honest, i don't know what PR does. lol.` / `you know how it goes.` Self-deprecation is common; performative positivity isn't.
- **Scheduling:** casual and concrete: `moved to monday at 11am` / `ok. noon target for videos.`

## Public channels

- **Status and handoffs:** state of the work first, then the link. `cc @[Name] @[Name]` on its own line, then what they'll finish and when. `@[Name] thumbnail if you need it`.
- **Asks to a group:** clear and practical, usually in a thread: `for future events, can they agree to a revision schedule?` / `what would be most helpful for you to have from us?`
- **Acks:** `yup.` / `looks great! thanks for your patience :slightly_smiling_face:` / `looks good to me` / `great! send it. thank you`
- **Blunt, but contained:** a short frustrated line is fine, then straight to the fix: `what a waste of effort. / yeah, let me put my local edit somewhere.` No speeches, no profanity in public.
- **Announcements:** a hook plus a link, sometimes a joke at their own expense: `if you want the highest quality viewing experience (and like roasting me)` plus the link. Not a press release.
- **Threads:** reply in the thread, one idea per message is common. Don't collapse several of their messages into one formal paragraph unless it's clearly a single status post.
- **Reactions:** a thumbs-up or a short `yup` often stands in for an ack. If a reaction would do, suggest that instead of drafting a paragraph.

## Recurring phrases

`yup.` · `yeah.` · `yeah!` · `word.` / `word, word.` / `woord.` · `for sure.` · `hiya` / `hiya team` · `looks great` · `looks good to me` · `thank you!` · `awesome thanks` · `hahah` / `ha!` · `lol` · `i'll be honest` · `you know how it goes` · `noice` · `gooot it` · `sorry for the late...` · `let me...` · `we're good with...` · `hollar if...` · `suss out`

## Not on Slack

On top of the `writing-voice` list: `Hi team,` / `Hi all,` · `Looping you in` (they write `cc`) · `Flagging this` · `Best,` or any signed close · emoji-heavy hype · long bullet memos · em dashes.

## Representative lines

Paraphrased and anonymized. Slashes mark separate messages or line breaks.

**DMs**

1. `yeah! put something on the books for next week. we'll jam.`
2. `yeah sorry. on the road, back in a bit`
3. `word, word.`
4. `for sure. / i'll be honest, i don't know what PR does. lol.`
5. `hiya. [Name] introduced me to someone who does PR in [an industry]. / do you see a need for a person in that space?`
6. `ok. noon target for videos. / and i'll do my best to have final exports by 10:30.`
7. `i'm trying real hard to have them uploaded the day before. / but you know how it goes.`

**Channels**

8. `video up, unlisted and embeddable.` plus the link
9. `looks great! thanks for your patience :slightly_smiling_face:`
10. `hiya team. great seeing you this weekend :slightly_smiling_face: / we're looking at ways to better arm partners with product and brand assets. / what does a banger brand kit look like for the way y'all work?`
11. `woord. thanks. sorry for the late add there.`
12. `yup. same directory.` plus the link
13. `@[Name] thumbnail if you need it`

## Drafting checklist

Run the `writing-voice` checklist, then:

1. Lowercase by default. No greeting, no sign-off.
2. As short as the moment allows. A complete ack can be `yup.` or `thank you!`. More to say? Two short messages beat one polished block.
3. Status or artifact first, link right there. Mentions are for routing, not decoration.
4. Match the room: fragments in close DMs, slightly more complete in public or partner channels, a hook plus link for company-wide posts.
5. Yes or no in their words: `yup`, `yeah`, `for sure`, `word`, or a plain `i won't be able to...`.
6. `hiya`, `word`, and `hahah` only when they fit the room and the mood. No forced slang, typo-play, or swearing.
7. Nothing from email: no `Hi [Name],`, no `Best,`, no em dashes, no "let me know if you have any questions."
8. Public drafts stay blunt but professional; sharper vents only in close DMs, and only when they'd actually go there.
9. If a reaction or a one-liner would do, draft that.

## Before and after

**Before (generic assistant draft, public channel):**

> Hi team, just circling back to confirm that the video is now uploaded and available. Please find the link below at your earliest convenience. Let me know if you have any questions!
> Thanks,
> [Name]

**After (public channel):**

> video up, unlisted and embeddable. [link]

**Before (generic assistant draft, colleague DM):**

> Hi [Name], Hope you're doing well. I wanted to flag that I may need to opt out of the working session this week due to a scheduling conflict. Please let me know if that works on your end.
> Thanks so much,
> [Name]

**After (colleague DM):**

> hey, i won't be able to drive a project that week.
> do i need to explicitly opt out?
