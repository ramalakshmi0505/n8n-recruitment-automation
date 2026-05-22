# n8n AI-Powered Recruitment Automation Pipeline

An end-to-end recruitment automation workflow built with [n8n](https://n8n.io). From job application to interview slot fully automated, zero manual steps.

---

## What It Does

| Step | Action |
|------|--------|
| 1 | Receives candidate application via Webhook |
| 2 | GPT-4o screens and scores the CV (0–100) |
| 3 | Routes to **Shortlist** or **Reject/Hold** |
| 4 | Shortlisted → schedules Google Calendar interview |
| 5 | Shortlisted → sends personalised interview invite email |
| 6 | Shortlisted → notifies hiring team on Slack |
| 7 | All candidates → logs to Airtable with AI evaluation |
| 8 | Rejected → sends polite rejection email |

**From application received to interview scheduled: under 30 seconds.**

---

## Architecture

```
Webhook (Application Form)
        │
        ▼
Validate Input
        │
        ▼
GPT-4o CV Screener
  (score, strengths, gaps, recommendation)
        │
        ▼
 ┌──────┴──────┐
 │             │
SHORTLIST    REJECT/HOLD
 │             │
 ├─ Google     ├─ Rejection
 │  Calendar   │  Email (Gmail)
 │             │
 ├─ Interview  └─ Save to
 │  Email         Airtable
 │
 ├─ Slack
 │  Notification
 │
 └─ Save to
    Airtable
```

---

## Tech Stack

- **n8n** — Workflow automation engine (self-hosted)
- **OpenAI GPT-4o** — CV screening and scoring
- **Gmail** — Candidate email communication
- **Google Calendar** — Automated interview scheduling
- **Slack** — Hiring team notifications
- **Airtable** — Candidate tracking CRM

---

## Getting Started

### Prerequisites

- n8n instance running (self-hosted or n8n Cloud)
- API credentials for: OpenAI, Gmail, Google Calendar, Slack, Airtable

### Import the Workflow

1. Open your n8n instance
2. Go to **Settings → Import Workflow**
3. Upload `workflow/recruitment_pipeline.json`
4. Connect your credentials for each node

### Configure

Update the following in the workflow:

```
YOUR_AIRTABLE_BASE_ID  →  Your actual Airtable Base ID
hr@yourcompany.com     →  Your HR email address
hiring-team            →  Your Slack channel name
```

### Webhook Payload

Send a POST request to the webhook URL with this body:

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "role": "Senior Cloud Engineer",
  "cv_text": "Full CV text here...",
  "linkedin_url": "https://linkedin.com/in/janedoe"
}
```

---

## AI Scoring Response

The GPT-4o screener returns a structured JSON evaluation:

```json
{
  "score": 82,
  "strengths": [
    "Strong Kubernetes and EKS experience",
    "Proven multi-region deployment track record",
    "Infrastructure automation with Terraform"
  ],
  "gaps": [
    "No JVM/Java experience mentioned",
    "Limited frontend exposure"
  ],
  "recommendation": "SHORTLIST",
  "summary": "Strong platform engineering background with 11 years AWS experience. Solid match for senior cloud roles."
}
```

Possible recommendations: `SHORTLIST` | `REJECT` | `HOLD`

---

## Folder Structure

```
n8n-recruitment-automation/
├── workflow/
│   └── recruitment_pipeline.json   # Import this into n8n
├── README.md
├── .gitignore
└── LICENSE
```

---

## Author

**Ramalakshmi Mani**  
Senior Cloud & Platform Engineer | AWS | Kubernetes | Terraform | n8n  
[LinkedIn](https://linkedin.com/in/ramalakshmim)

---

## License

MIT License — free to use and modify.
