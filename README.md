# Guest Outreach & Booking Pipeline

Prepared by: TheAiGeng
Contact for install help: https://x.com/TheAiGeng

Overview
A reusable pipeline for scaled guest outreach that ingests lead lists (CSV, Sheets, Airtable), enriches leads, generates three personalized email variants per lead, sends staged emails with follow-ups via SMTP, handles bookings with Calendly, and creates episode briefs in Notion. Includes deliverability safeguards and logging.

What's included
- workflows/guest_outreach_pipeline.json  (n8n import file)
- samples/sample_leads.csv
- templates/outreach_email_stage1.txt
- templates/outreach_email_stage2.txt
- templates/episode_brief_template.txt
- scripts/smtp_settings_stub.json
- README.md
- .gitignore

Before you start (what you need)
- An n8n instance (cloud or local). Local requires Docker.
- SMTP account for sending emails (use a dedicated domain or subdomain). Do NOT put credentials in the repo.
- Calendly account and API token (for auto-creating events) or use invite links in emails.
- Notion account and database for episode briefs (optional but recommended).
- Optional: Lead enrichment API (Clearbit, Apollo) for audience size and socials.
- Recommended: ask IT to verify DKIM/SPF for the sending domain before large sends.

Step-by-step setup (non-technical)
1. Import the workflow: open n8n → Import → Import from File → choose workflows/guest_outreach_pipeline.json.
2. Add credentials in n8n Credentials: SMTP (or Office365), OpenAI (optional), Calendly (optional), Notion (optional). Keep keys private.
3. Upload sample leads or point the Read CSV node to your leads file. Replace the sample CSV path if needed.
4. Update environment variables or node fields: SMTP_FROM, CALENDLY_EVENT_TYPE, FOLLOWUP_DAYS, LEADS_CSV_PATH.
5. Run the workflow in manual/test mode for one or two leads. Verify emails send, follow-up triggers after the wait period, Calendly event gets created on acceptance (if enabled), and Notion brief appears.

Deliverability & safety checklist (do NOT skip before a mass send)
- Verify DKIM & SPF for the sending domain with your IT team.
- Warm up the sending domain and IP before large campaigns (start small, increase volume gradually).
- Use per-campaign throttling in n8n (set a pause between sends) and enable bounce handling in your SMTP provider.
- Have a human approval gate before any list over 500 contacts.
- Monitor bounces and unsubscribe requests and remove addresses immediately.

Testing tips
- Start with 2–3 test leads (use sample_leads.csv).
- Use a test SMTP provider or a mailbox you control to confirm deliverability and formatting.
- Verify that Calendly invitation links are included and Notion briefs are created per booking.

Acceptance criteria (how to know it's working)
- For test leads: personalized emails are generated and delivered, follow-up email sends after the configured delay, Calendly booking results in a Notion brief creation, and outreach_log.csv records each send.
- For pilot campaign: consistent send success rate (>98%) and no more than 1% hard bounces after warm-up.

Quick GitHub upload (5 lines)
1. Go to github.com and sign in, click + → New repository, name it `guest-outreach-pipeline` and create it.
2. Click Add file → Upload files, drag-and-drop the unzipped project folder contents (all files and folders).
3. Confirm no `.env` or secret files are included, enter commit message `Initial upload: guest outreach pipeline` and Commit changes.
4. Optional: create a Release and attach the zip for easy download.
5. Share the repo link with your integrator or post to https://x.com/TheAiGeng for help.

License: MIT
