# Email Handling Instructions

1. For any email task in this repo, first read and follow `skills/email-attention-flow/SKILL.md`. Also use `email_templates.md` and `log.md` when relevant.
2. If the user asks you to scan for emails that are meant for them, be diligent about checking all relevant addresses and aliases. Surface messages that are personal to the user or clearly require action, and filter out marketing, list mail, and generic announcements unless the user asks for them.
3. Do not surface generic emails that are not personal to the user or do not require action unless the user explicitly asks for them.
4. Filter out emails that `log.md` shows were already handled. If the user tells you to mark off an email, record it in `log.md` and do not surface it again. If you send an email for the user, add it to `log.md`.
5. By default, if the user asks you to respond, draft, or reply and has not explicitly authorized immediate sending, do this for each email:
   - show an `EMAIL` block with the relevant incoming email
   - show a `DRAFT` block with the proposed response
   - ask for confirmation before sending
   - instructions such as `say`, `reply`, `respond`, `forward`, `decline`, `tell them`, or `come up with a response` are drafting instructions only and are not permission to send
   - only explicit send authorization such as `send`, `email them`, `forward it now`, `do it`, or `send all standard templates` allows immediate sending
   - if a mixed request contains some items with explicit send authorization and others without it, send only the explicitly authorized subset and draft the rest
6. If the initial scan finds emails that match one of the categories in `email_templates.md`, group them together and propose using the standard template. If the user tells you to do that, send those standard-template replies directly; for those cases you do not need to show drafts first.
7. If you send emails, always include the Gmail link to each email you sent.
8. For recall-sensitive scans such as "surface all emails requiring my attention from the last X days/weeks", do not rely on a single Gmail search. Use a recall-first process that checks all relevant addresses, covers the whole timeframe, verifies against `log.md`, and explicitly calls out encrypted or ambiguous personal emails instead of silently dropping them.
9. If the user responds only about some of the surfaced emails, do not lose track of the others. Keep a visible residual queue of actionable emails until they are handled, deferred, or marked off.
