# Call Analysis Automation

> Status: in development — portfolio project.

AI-powered system that turns sales call recordings into structured CRM data:
transcription, client intent scoring, and consent-based follow-up — fully
automated with n8n and Google Gemini.

## Overview

Sales teams lose time manually listening to calls and filling CRM fields.
This project automates the loop: a manager drops a call recording into
Google Drive, and the workflow transcribes it, extracts deal-relevant data,
logs everything to Google Sheets, and triggers the appropriate follow-up
(email or reminder) based on the client's consent.

Tested on synthetic recordings to keep real client data private.

## Features

- End-to-end pipeline: from a recording in Google Drive to a structured CRM row.
- Full call transcription and a concise summary.
- Extraction of 14 structured fields: client, company, product, budget,
  total offer, phone, call type, interest level, purchase probability,
  client status, next step, tags, and more.
- Deterministic phone-number guardrail: the LLM reports the number exactly
  as heard; JavaScript validates format and flags uncertain rows for manual
  review instead of silently "fixing" them.
- Consent-based follow-up: three email scenarios or a reminder.
- Privacy-first testing on synthetic call data.

## Technologies Used

| Component | Technology |
|---|---|
| Workflow automation | n8n (self-hosted) |
| Speech understanding & extraction | Google Gemini API (`gemini-3.5-flash`) |
| Recording storage | Google Drive |
| CRM log | Google Sheets |
| Follow-up emails | Gmail |
| Data processing & validation | JavaScript (n8n Code node) |

## Workflow Architecture

```
Google Drive (new recording)
        |
        v
[Trigger] -> [Download] -> [Gemini: Analyze Audio] -> [Code: parse & validate]
                                                            |
                                                            v
                                                      Google Sheets
                                                            |
                                          consent? ---------+--------- no consent?
                                            |                             |
                                            v                             v
                                     Gmail follow-up                Reminder / next step
```

### Design decisions

- **LLM perceives, code validates.** Digit counting, format checks, and
  normalization are deterministic and live in JavaScript, not in the prompt.
- **Honest pass-through.** The model is forbidden from truncating or padding
  phone numbers; suspicious values are flagged, never invented.
- **Output sanitization.** The Code node strips Markdown fences and extracts
  the JSON body, so formatting quirks of the LLM cannot break the pipeline.
- **Model selection under constraints.** `gemini-3.5-flash` chosen for the
  free tier: stable, available, and strong enough for audio extraction.

## Current Limitations

- Recordings are uploaded manually (no telephony integration yet).
- The client contact table is maintained manually.
- Runs on the Gemini free tier: rate limits apply, and model availability
  changes over time.
- Uncertain phone numbers go to manual review rather than being auto-corrected.
- Current tests cover Russian-language calls only.

## Future Improvements

- Automatic call ingestion from telephony or CRM.
- Two-stage extraction: dedicated transcription model, then LLM structuring.
- Review queue for flagged rows and an analytics dashboard.
- Python + FastAPI backend for custom processing and API access.
