# JobPilot AI – Intelligent Job Application Automation System

JobPilot AI is an AI-powered job application automation workflow built with **n8n and Groq**.

The system allows a candidate to submit their job application details once. It automatically analyzes the CV against the job description, generates an AI fit score and personalized cover letter, emails the result, logs the application in Google Sheets, and creates a Google Calendar follow-up reminder after 7 days.

## Features

- Collect applicant details through an n8n Form
- Accept CV / Resume text and Job Description
- Analyze CV-job compatibility using Groq AI
- Generate a 0–100 fit score
- Provide AI reasoning for the score
- Identify candidate strengths and gaps
- Generate a personalized cover letter
- Email the result using Gmail
- Log application details in Google Sheets
- Create a 7-day follow-up reminder in Google Calendar
- Send a fallback email if the Groq AI request fails
- Fully automate the workflow after form submission

## Tech Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation |
| Groq API | AI-powered CV analysis and cover letter generation |
| Llama 3.3 70B | Large Language Model |
| Gmail | Email automation |
| Google Sheets | Application logging |
| Google Calendar | Follow-up reminders |
| JavaScript | Data preparation and AI response processing |

## Workflow

```text
Applicant
   ↓
n8n Form Trigger
   ↓
Prepare AI Request
   ↓
Groq AI
   ↓
Parse AI Result
   ↓
AI Success?
   ├── TRUE
   │    ↓
   │  Gmail → Google Sheets → Google Calendar
   │                              ↓
   │                         7-Day Follow-up
   │
   └── FALSE
        ↓
    Fallback Email
```

## Form Inputs

| Field | Type | Required |
|---|---|---|
| Applicant Name | Text | Yes |
| Applicant Email | Email | Yes |
| Job Title | Text | Yes |
| Company Name | Text | Yes |
| Job Description | Textarea | Yes |
| CV / Resume Text | Textarea | Yes |

## AI Output

Groq analyzes the CV against the job description and returns:

```json
{
  "fit_score": 85,
  "reasoning": "The candidate has strong Python and automation skills that match the main requirements.",
  "strengths": [
    "Python",
    "Git",
    "Automation"
  ],
  "gaps": [
    "Limited professional experience"
  ],
  "cover_letter": "Dear Hiring Manager..."
}
```

## Google Sheets Setup

Create a spreadsheet named:

```text
Job Application Autopilot
```

Create a tab named:

```text
Applications
```

Use these headers in Row 1:

```text
Submitted At
Applicant Name
Applicant Email
Job Title
Company
Fit Score
Reasoning
Strengths
Gaps
Cover Letter
```

Each successful application is automatically added as a new row.

## Google Calendar

The workflow creates a calendar event 7 days after the application.

Example:

```text
Title:
Follow up: Python Developer at ABC Technologies

Duration:
30 minutes

Reminder:
7 days after application
```

## Error Handling

If the Groq API fails, the workflow detects the error and sends a fallback email instead of silently stopping.

```text
Groq API
   ↓
Error
   ↓
Parse AI Result
   ↓
AI Success? = FALSE
   ↓
Fallback Email
```

## Required Credentials

Configure these credentials in n8n:

### Groq API

Create a Groq API key and configure it as an HTTP Header Auth credential.

```text
Authorization: Bearer YOUR_GROQ_API_KEY
```

API endpoint:

```text
https://api.groq.com/openai/v1/chat/completions
```

### Gmail

Connect the Google account that should send the applicant emails.

### Google Sheets

Connect the Google account that owns or has access to the application tracking spreadsheet.

### Google Calendar

Connect the Google account whose calendar should contain the 7-day follow-up reminders.

For a simple setup, the same Google account can be used for Gmail, Google Sheets, and Google Calendar.

## Installation

### 1. Import the workflow

Open n8n and choose:

```text
Workflows → Import from File
```

Select:

```text
job_application_autopilot_groq.json
```

### 2. Configure Groq

Open the **Groq AI** HTTP Request node.

Set:

```text
Method: POST
URL: https://api.groq.com/openai/v1/chat/completions
```

Select your Groq Header Auth credential.

### 3. Configure Gmail

Open:

- Email Cover Letter
- Fallback Email

Connect your Gmail OAuth2 credential.

### 4. Configure Google Sheets

Create the spreadsheet and `Applications` tab described above.

Connect Google Sheets OAuth2 and select the spreadsheet and sheet.

### 5. Configure Google Calendar

Open **7-Day Follow-up Reminder**, connect Google Calendar OAuth2, and select your Primary Calendar.

### 6. Test

Submit sample information through the n8n Form Trigger test URL.

Verify:

- Groq generates the fit score
- Cover letter is generated
- Gmail sends the result
- Google Sheets receives a new row
- Google Calendar creates the follow-up event

## Sample Test Data

### Applicant Name

```text
Rahul Kumar
```

### Applicant Email

```text
your-email@gmail.com
```

### Job Title

```text
Python Developer
```

### Company Name

```text
ABC Technologies
```

### Job Description

```text
We are looking for a Python Developer with knowledge of
Python, REST APIs, SQL, Git and automation. Freshers with
strong programming skills are welcome to apply.
```

### CV / Resume Text

```text
Rahul Kumar

B.Tech Computer Science graduate.

Technical Skills:
Python
Java
JavaScript
HTML
CSS
React
Git
GitHub

Projects:
Developed an online exam proctoring system using Python.
Built an AI-powered automobile recommendation system.
```

## Expected Result

```text
Application Submitted
        ↓
CV analyzed by Groq
        ↓
Fit Score Generated
        ↓
Personalized Cover Letter Generated
        ↓
Email Sent
        ↓
Application Logged
        ↓
7-Day Follow-up Created
```

## Project Structure

```text
JobPilot-AI/
│
├── job_application_autopilot_groq.json
└── README.md
```

## Future Improvements

- Upload CV as PDF instead of pasting resume text
- Automatically extract job descriptions from URLs
- Add application status tracking
- Add interview preparation questions
- Add automated follow-up emails
- Store resumes in Google Drive
- Add Telegram or WhatsApp notifications
- Build an application analytics dashboard
- Add multiple AI agents

## Project Objective

The objective of JobPilot AI is to demonstrate how **Generative AI and workflow automation** can be combined to automate repetitive tasks in the job application process.

The project demonstrates:

- AI integration
- Prompt engineering
- API integration
- Workflow automation
- Conditional logic
- Error handling
- Email automation
- Spreadsheet automation
- Calendar automation

## Project Summary

**JobPilot AI** follows a simple principle:

```text
Submit Once
     ↓
AI Analyzes
     ↓
Cover Letter Generated
     ↓
Email Sent
     ↓
Application Logged
     ↓
Follow-up Scheduled
```

> **One submission. Complete job application assistance.**

## Author

**Your Name**

B.Tech – Computer Science and Engineering

## License

This project is created for educational and portfolio purposes.
