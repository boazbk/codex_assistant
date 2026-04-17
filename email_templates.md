# Email Templates

Use these templates for standard request types recognized during an inbox scan.

Before using them, customize the placeholders below to match the user's environment:

- `{first_name}`: recipient's first name
- `{user_name}`: the sender name the user wants to sign with
- `{organization_name}`: institution, company, lab, or team name
- `{program_info_url}`: admissions or program page
- `{opportunities_url}`: page listing jobs, internships, or openings
- `{reference_form_url}`: intake form for recommendation-letter requests
- `{reference_alias_email}`: mailbox to use for letter submissions
- `{admin_contact}`: assistant or coordinator to cc when relevant
- `{course_name}`: course, seminar, or program name
- `{course_info_url}`: course or program webpage
- `{resource_1}`, `{resource_2}`, `{resource_3}`: optional beginner-friendly resources

Workflow rule:

- If a scan finds emails that match one of the categories below, group them together and propose using the standard template.
- Only if the user explicitly tells you to send the standard-template replies, send those replies directly without showing individual drafts first.
- Approval of wording alone is not authorization to send.
- Replace placeholders before sending.

Priority notes:

- `EARLY-CAREER STUDENT INQUIRY` overrides the generic `NO OPEN POSITIONS` template when applicable.
- `RECOMMENDATION LETTER NEXT STEPS` is only for cases where the user has agreed to write the letter.

## GENERIC ACKNOWLEDGMENT

Use when the message deserves a quick personal response, but the user mainly needs to acknowledge receipt and buy time for a fuller follow-up.

```text
Hi {first_name},

Thank you for your note. I received it and will follow up after I have had a chance to review the details more carefully.

If there is a deadline I should know about, feel free to send it along.

Best,
{user_name}
```

## GRAD PROGRAM INQUIRY

Use when the sender is asking about graduate study, a formal application process, or admissions to a program associated with the user.

```text
Dear {first_name},

Thank you for your interest in {organization_name}.

Applications are handled through the formal admissions process rather than by individual faculty members. You can find more information here:
{program_info_url}

Best,
{user_name}
```
