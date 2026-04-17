---
name: email-attention-flow
description: Use for recall-sensitive scans of personal Gmail that must surface all emails the user personally owes a response to over a requested timeframe, across all relevant addresses, while still capturing other actionable mail, filtering bulk and list mail, honoring log.md, proposing standard-template replies from email_templates.md, defaulting to per-email confirmation drafts before sending, and always returning Gmail links after send.
---

# Email Attention Flow

Use this skill for inbox-attention scans, reply-needed audits, and email-response workflows in this repo.

## Required Local Files

- Read `AGENTS.md` first.
- Read `log.md` before surfacing or replying to anything.
- Read `email_templates.md` when the scan may include standard request types.

## Core Goal

Maximize recall for personal threads where the user is plausibly the next actor, especially emails they personally owe a reply to. Prefer a few false positives over a false negative on personal-response coverage, while still filtering obvious bulk and list noise. If there is meaningful doubt whether the user still owes a response, include the item in a lower-confidence bucket instead of silently dropping it.

## Scan Workflow

1. Start with a broad all-mail search for the full requested timeframe. Do not assume the inbox alone contains every thread the user still owes a response to.
2. Run a second pass that excludes obvious bulk categories such as promotions, forums, social, and updates to build a human-origin shortlist.
3. Run an additional pass tuned for personal reply-needed mail. Favor searches and filters that catch direct human conversation, recent back-and-forth, and mail addressed to one of the user's known or visible addresses.
4. If the timeframe is more than a few days, do not rely only on the first page of results. Paginate or search by time slices so older actionable mail is not missed.
5. Use `search_email_ids` or equivalent coverage checks to verify the result set is not suspiciously sparse.
6. Read bodies for shortlisted messages whose snippets are insufficient, and read full threads when the latest responder or next actor is ambiguous.
7. Check all relevant addresses visible in the Gmail results, prior handled threads, and message headers. Do not assume one address is enough.
8. For each shortlisted personal thread, explicitly classify whether the user owes a reply, owes some other action, is waiting on the other side, or the thread is unclear.
9. Remove anything already logged in `log.md` as handled or marked off.
10. Surface encrypted or otherwise unreadable personal emails as unresolved items rather than ignoring them.

## Response-Owed Classification

For each shortlisted personal thread, assign one of these states before deciding whether to surface it:

- `Owes reply now`: the latest meaningful state of the thread suggests the user should respond
- `Other action owed`: the user likely needs to do something, but not mainly send a reply
- `Waiting on them`: the latest ball is in the other side's court
- `Handled / closed`: no further action appears needed
- `Unclear`: not enough evidence; inspect more and surface if still ambiguous

Use these rules:

- If the latest human message to the user contains a direct ask, decision request, invitation, scheduling prompt, reminder, or follow-up and there is no later clear answer from the user, treat it as `Owes reply now`.
- Do not treat the mere existence of a sent reply from the user as closure. If the reply only acknowledged receipt, answered part of the message, deferred without resolution, or left one of the asks untouched, inspect the thread further and keep it surfaced when appropriate.
- If the user was the last sender but promised a later response, follow-up, introduction, document, decision, or other substantive next step that does not appear to have been fulfilled yet, still treat the thread as `Owes reply now`.
- If the user was the last sender and asked the other side for information, availability, confirmation, or a deliverable, usually classify as `Waiting on them`, not `Owes reply now`.
- Distinguish `I owe them a follow-up` from `I am waiting on them`. A latest outgoing message such as `I'll reply later`, `I'll send thoughts soon`, `let me get back to you`, or `I'll follow up tomorrow` means the thread can still require the user even if they sent the last email.
- If the latest inbound message is only a thanks, acknowledgement, or FYI with no remaining ask, usually classify as `Handled / closed` unless an earlier unresolved request is still outstanding.
- If a thread has multiple recipients, surface it when the message is clearly addressed to the user personally or the user is an expected responder, even if others were copied.
- If the thread is personal but the latest state is hard to infer from snippets, read the latest message body and enough prior context to determine who the next actor is.
- If you still cannot confidently tell whether the user owes a response, keep it in `Lower confidence / unresolved` rather than dropping it.

## What Counts As "Personally Owes a Response"

Include:

- Direct asks, decisions, approvals, confirmations, and requests for a meeting or reply
- Threads where the user explicitly promised a later reply or follow-up that still appears outstanding
- Requests to review a document, proposal, article, form, or application
- Personal scheduling items where the user is the next actor
- Emails that appear individually addressed or clearly meant for the user even if sent to one of several addresses

Also keep a separate actionable bucket for:

- Administrative items that require the user to click approve, sign up, vote, confirm, or submit something even when a written reply is not the main action

Exclude unless the user asks for them:

- Marketing, newsletters, list mail, ordinary announcements, and generic broadcast mail
- FYI-only replies where the user is clearly not the next actor
- Messages that `log.md` says were already handled

When uncertain:

- Keep the item in a lower-confidence or unresolved section
- Explain briefly why it might still need attention

## Non-Miss Guardrails

- Never rely on a single Gmail query for a recall-sensitive scan.
- Never assume `in:inbox` is a complete proxy for outstanding personal replies; archived threads can still require the user.
- Never stop after handling only the emails the user explicitly mentions if other actionable items remain from the same scan.
- Keep a visible residual queue of still-open emails until they are handled, deferred by the user, or added to `log.md`.
- When a latest message is from the user or an obvious acknowledgement, verify whether the thread is actually done before surfacing it.
- When the user specifically asks for emails they owe a response to, optimize for that question first. Do not let non-reply admin tasks crowd out reply-needed threads.
- When the user is the last sender, check for promised follow-ups before classifying the thread as `Waiting on them` or closed.
- Administrative mail can be actionable even when it looks routine; check the latest message body before downgrading it.

## Output For Scans

Prefer this structure:

- Scope and coverage: exact timeframe, mailbox scope, and whether you verified older slices or pagination
- `Owes reply now`: high-confidence threads where the user is the next responder
- `Standard-template candidates`: grouped items that match `email_templates.md`
- `Other action owed`: actionable items where the main next step is not a reply
- `Waiting on them / likely closed`: useful when needed to explain why personal threads were excluded
- `Lower confidence / unresolved`: ambiguous personal items, encrypted mail, or cases where context is incomplete
- `Already handled`: only when useful to explain why something was excluded

## Response Workflow

Default behavior when the user asks to respond, draft, or reply:

1. Do not send immediately unless the user explicitly authorizes sending.
2. For each email, show two blocks:
   - an `EMAIL` block with sender, subject, and the relevant latest text
   - a `DRAFT` block with the proposed reply
3. Ask for confirmation before sending.
4. Treat instructions such as `say`, `reply`, `respond`, `forward`, `decline`, `tell them`, or `come up with a response` as drafting instructions only. These do not authorize sending.
5. Treat only explicit commands such as `send`, `email them`, `forward it now`, `do it`, or `send all standard templates` as authorization to send.
6. In a mixed batch, send only the subset the user explicitly authorized to send. Keep the rest in `EMAIL` and `DRAFT` blocks.

Exception:

- If the user explicitly tells you to send one of the standard templates from `email_templates.md` (for example `send all standard templates`), you may send those template-based replies directly without showing individual drafts first.

## Standard Templates

- Read `email_templates.md`.
- During the initial scan, actively look for emails that match those categories.
- Group matching emails together and propose the shared template rather than drafting each from scratch.
- Only send standard-template replies directly when the user explicitly says to send them.

## After Sending

- Always include the Gmail link to every sent email in the response.
- Append handled emails to `log.md`.
- If some actionable emails remain unsent, say so explicitly.
