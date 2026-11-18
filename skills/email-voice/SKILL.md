---
name: email-voice
description: The user's email voice, built from their real sent mail. Use when drafting, rewriting, or reviewing an email, reply, or forward on the user's behalf. Loads the writing-voice skill first, then adds email-only rules - greetings and sign-offs, two capitalization registers, length and structure, registers by audience (close partners, internal billing, vendors, new contacts, formal or legal, difficult threads), scheduling, recurring phrases, a drafting checklist, and before/after examples.
---

# Email voice

> Draft skill. Review it before relying on it.

**Load `writing-voice` first** (in this plugin, `../writing-voice/SKILL.md`). It holds the base tone rules and the phrases to avoid. This skill adds only what's specific to email; where they differ, this skill wins for email.

Email is more composed than Slack and is often signed. Don't carry Slack habits (no sign-off, fragments everywhere, `word.`) into email, or email habits into Slack.

## Configuration

| Parameter | What it is |
| --- | --- |
| `{casual-name}` | The short name or nickname they sign casual mail with; often lowercase in casual threads |
| `{first-name}` | Their first name, for vendor and slightly formal threads |
| `{full-name}` and `{company}` | For formal first contact and legal or licensing mail only |

Take these from your memory or the user's configuration. Don't write them into shared files.

## Greetings

Use one, or none.

- **No greeting** is common, especially in a thread that's already moving or a reply of one to three sentences.
- `Hey [Name],` is the default when they greet someone; sometimes with a period: `Hey [Name].`
- `Hi [Name],` for a slightly more composed vendor or intro note.
- `Hello,` is rare, for formal cold outreach only.
- Warm first contact can be `Nice to meet you [Name]!`
- Sometimes just the name on its own line, or the name worked into the first sentence: `[Name], thank you for the quick response.`
- Never `Hi team,` to open, `Dear`, or "hope this finds you well."

## Sign-offs

- `{casual-name}` is the default close; lowercase when the rest of the mail is casual.
- `{first-name}` for vendor and supplier threads that are a bit more formal (orders, warranties, declines, firm follow-ups).
- `{full-name}` plus `{company}` only for formal first contact or legal and licensing mail.
- `Thanks,`, `Thank you,`, or `Thanks!` often sit on their own line above the name. An occasional `all the best,`.
- Short replies, especially lowercase ones, often have no sign-off at all.
- Never `Best,`, `Best regards,`, `Kind regards,`, `Warmly,`, or `Thanks in advance`.

## Two registers

- **Composed:** complete, normally capitalized sentences for new or formal recipients, careful declines, and first notes to suppliers.
- **Casual:** lowercase, looser, a bit run-on, for people they already write to regularly (familiar partners, internal billing, quick support). About a third of their mail starts lowercase.

If the thread is already casual, stay casual. If it's a first note or a careful decline, capitalize normally.

## Punctuation and formatting

- Short paragraphs with plenty of line breaks: five two-line paragraphs over one dense block.
- Periods on full sentences; question marks on questions. Exclamation marks only for genuine warmth.
- Occasional ellipses. Em dashes are rare; prefer a period or a comma.
- Emphasis is occasional `*asterisks*` or caps on one word (`I LOVE the design.`).
- Dashes or numbers only for real requirements, order details, or contract edits.
- Casual mail has light typos and shortcuts (`thru`, `bc`). Don't invent typos in a draft, and don't correct a casual thread into formal English.

## Length and structure

- **Quick acks and in-thread replies:** one to three sentences, no preamble.
- **Typical partner or vendor note:** optional greeting, the point, one piece of context or a next step, thanks, sign-off. Usually under a short screenful.
- **Longer only when needed** (contract edits, a hard pushback, a structured ask), and still short paragraphs and bullets, not a memo.
- **Internal forwards** (billing, events): one to three lines of context, then the forwarded message. Don't restate the invoice in prose.

## Emoji

Sparing. A `:)`, a facepalm, a thumbs-up, or an occasional sweat-smile when the note is warm or self-effacing. Never a row of emoji, and never in formal, legal, or firm follow-up mail.

## Registers by audience

| Audience | Register |
| --- | --- |
| Close partners they already joke with | Lowercase, a little slang, short. `{casual-name}` or no sign-off. In-jokes and playful openers stay inside that circle. |
| Internal billing, events, finance | Short and practical. Lowercase is fine. One or two lines of context, the due date if it matters, no fluff. |
| Vendors and suppliers | `Hey` or `Hi` plus first name, normal capitalization, `Thank you,` and `{first-name}`. Friendly, not stiff. |
| Warm intros, candidates, new contacts | Complete sentences, real warmth, still short. Honest about what they don't know. |
| Formal, legal, or licensing first contact | `Hello,` or `Hi [Name],`, proper capitalization, `{full-name}` and `{company}`. Plain English, not legalese. No jokes. |
| Difficult or late threads | Firmer, still short. Name the need and the alternative. `{first-name}` sign-off is fine. |

## How they say common things

- **Yes:** direct, then the plan. "Yes, let's go with your plan for..." / "Everything sounds good to me." / "We'll proceed with..." With close partners: "let's run it." / "heck yeah." / "yup. just sent"
- **No:** warm and clear, often with a door left open. "Unfortunately, that date won't work. Thank you for the response, though. If we need another round in the future, I'll reach out." Casual: "no thanks." plus a one-line reason.
- **Delay:** honest and practical: "i didn't get to this before I was out. happy to pick it back up when i'm back."
- **Thanks:** "thanks!" / "Thanks for the confirmation." / "Thank you for the follow-up [Name]." / "Thanks for your patience."
- **Asks:** straight. "Could you please..." / "Do you have a [document] I could share with the team?" / "Just wanted to check in on progress." In a late thread, blunt: "Do you have an update on this?"
- **Pushback:** human and specific, short sentences, first person. Name the problem and the remedy, acknowledge the other side ("I understand what you're saying."), and hold the line.
- **Delegating:** a plain intro, then step out of the way. "Hey team, wanted to send this along." / "thanks! i forwarded your email to our events team." / "[Name], can you provide the shipping info for this order?"

## Scheduling

- Days and times are casual: "Let's do friday! I'll send a calendar invite. Move it around if needed."
- Time zones as `ET` / `PT`, sometimes combined: `Thursday @ 11ET/2PT`.
- Offer alternatives instead of demanding a slot: "also free at 11 and 4 ET, if those work better."
- "do you have a scheduling link?" is a normal ask.

## Recurring phrases

`just wanted to check in` · `wanted to send this along` · `thanks for your patience` · `let me know` · `we'll proceed` · `path to move forward` · `reach out` (in a sentence, not as a closer) · `thru` · `stoked` · `heck yeah` · `dig` · `btw` · `I'm excited` · `I LOVE` · `hmu` (close partners only)

## Representative lines

Paraphrased and anonymized. Slashes mark line breaks.

1. `Hey [Name]. Could you please downgrade my account? I'd upgraded it at a teammate's request but it turns out we don't need it. Thanks! / {casual-name}`
2. `oh man. sorry to have dragged us through this :facepalm: / hope your day's going well :)`
3. `Thank you [Name] for the clarity and the path to move forward. / Yes, let's go with your plan. / I'll be available for a quick review by [date]. / {casual-name}`
4. `[Name], / We need to resolve this. / If we can't get [the deliverable], we'll need to work out another arrangement. / {first-name}`
5. `Unfortunately, that date won't work. Thank you for the response, though. / If we need another batch for a future event, I'll reach out. / {first-name}`
6. `Hey team, wanted to send this along. / I have no existing relationship with them but figured you'd be able to filter more quickly than me.`
7. `did this invoice come thru to you already? / if not, it's due tomorrow.`
8. `hey [Name], set up a shared channel thursday. hmu if there's any issue with the invite. / looking forward to working together! / {casual-name}`
9. `I'll be honest, I have no idea how that process works. I just get brought in for vibe checks 😅`
10. `This is terrific! / Thank you. Good to post 👍`

## Drafting checklist

Run the `writing-voice` checklist, then:

1. Pick the register from the audience table. Close partner: lowercase, short, `{casual-name}` or no sign-off. Vendor or careful decline: normal capitalization, `Hey` or `Hi`, `{first-name}`. Formal first contact only: `{full-name}` and `{company}`.
2. Greeting or not? In a moving thread or a short reply, usually not.
3. Sign off the way they would, or not at all on a tiny reply.
4. Short paragraphs; bullets only for real requirements.
5. Mention the due date or next step if there is one; don't recap the thread.
6. For billing or internal forwards: one to three lines of context, then the forward.
7. Nothing from Slack: no `word.`, no lowercase fragments to a new vendor.

## Before and after

**Before (generic assistant draft):**

> Hi [Name],
> Hope this email finds you well. I wanted to quickly circle back regarding the draft we discussed last week. Please find the updated cut at the link below at your earliest convenience. Please don't hesitate to reach out if you have any questions or concerns.
> Best regards,
> {full-name}

**After (familiar partner):**

> Hey [Name],
> updated cut is here: [link]
> take a look when you can and let me know if you want changes.
>
> thanks,
> {casual-name}

**After (vendor, slightly more formal):**

> Hi [Name],
> Thanks for your patience. The updated cut is here: [link]
> Let me know if you'd like any changes.
>
> Thank you,
> {first-name}
