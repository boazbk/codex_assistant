# codex_assistant

This repository is a public template for running a Codex-based email triage and response workflow. It keeps the core pieces of the private setup intact while stripping out personal names, addresses, institutions, and historical email activity.

## What Is Included

- `AGENTS.md`: top-level operating rules for how the assistant should scan, draft, send, and track email work
- `skills/email-attention-flow/SKILL.md`: the detailed recall-first workflow for email-attention scans
- `email_templates.md`: reusable standard-response templates with placeholders
- `log.md`: an intentionally blank action log that the assistant can use to avoid resurfacing already handled threads

## How To Customize It

1. Update `email_templates.md` with your own signature, links, aliases, and standard response categories.
2. Edit `AGENTS.md` to reflect your own approval policy, what counts as actionable mail, and any rules about sending or logging.
3. Tune `skills/email-attention-flow/SKILL.md` if your workflow needs different scan heuristics, mailbox coverage rules, or output buckets.
4. Keep `log.md` local to your usage. The checked-in file is a blank template; if you plan to publish your fork, clear it before pushing.
5. If you use multiple addresses, make that explicit in `AGENTS.md` so the assistant knows to search across them rather than only the inbox default.

## Recommended Workflow

Open Codex in this repository so the local instructions and skill files are in scope. Then use prompts that tell the assistant:

- what time window to scan
- whether you want a draft-only or send-authorized workflow
- whether standard templates should be grouped and proposed
- whether handled items should be written to `log.md`

## Example Prompts

The prompts below are redacted composites based on recent real workflows from the private source repo.

- `Scan all personal emails from the last 7 days that still require my response. Check all relevant addresses, exclude newsletters and generic announcements, and filter anything already listed in log.md.`
- `Group any emails that match a standard template, then draft custom replies for the rest. Do not send anything yet.`
- `Use the standard no-open-positions template for these three inquiries, but wait for my confirmation before sending.`
- `Draft a reply to the speaking invitation that politely declines because of scheduling constraints, and keep the rest of the actionable queue visible.`
- `Mark the administrative reminder and the committee-vote email as handled in log.md so they do not resurface next time.`
- `After I approve the drafts, send the approved replies and include Gmail links for each sent message.`
- `Surface lower-confidence personal emails too if there is any chance I still owe a follow-up.`

## Notes

This template is optimized for high-recall personal email workflows rather than generic inbox zero cleanup. The intent is to avoid false negatives on direct human emails that still need the user's attention, while still filtering out bulk mail and keeping a visible residual queue until everything is handled.
