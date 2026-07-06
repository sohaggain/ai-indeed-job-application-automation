# AI Indeed Job Application Automation

An n8n workflow that automatically discovers, scores, and drafts cover letters for relevant job listings on Indeed — built by [Sohag Gain](https://bd.linkedin.com/in/sohaggain), Founder & CEO of [AI Smart Galaxy](https://www.aismartgalaxy.com).

This project demonstrates a real-world AI agent pipeline combining a web scraper, two LLM reasoning steps, and structured output delivery to Google Sheets — the same class of automation AI Smart Galaxy builds for clients (lead qualification, CRM enrichment, GoHighLevel workflows, etc.), applied here to solve the founder's own job/opportunity-sourcing problem.

## What It Does

1. **Schedule Trigger** — runs on a recurring schedule (e.g. daily) with no manual intervention.
2. **Run an Actor (Apify)** — calls the "Indeed Jobs Scraper" Apify actor with a configurable query (e.g. `AI Automation`, remote, US, last 3 days) and pulls fresh listings.
3. **Get Dataset Items** — retrieves the scraped job records from the Apify dataset.
4. **Limit** — caps the batch size during testing/development to control API costs (removed before running live).
5. **OpenAI1 (GPT-5-mini) — Job Fit Scoring** — scores each listing 0–10 against a candidate resume/skill profile using a weighted rubric:
   - Resume/skill match (up to 4 pts)
   - Compensation threshold — $25/hr, $4,000/mo, or $50,000/yr (3 pts)
   - Remote status (1 pt)
   - Benefits offered (1 pt)
   - Full-time status (1 pt)
6. **OpenAI2 (GPT-4o-mini) — Cover Letter Generation** — drafts a tailored cover letter per job based on the listing details and the candidate's resume.
7. **Google Sheets** — appends or updates each job (matched by URL to avoid duplicates) into a tracking sheet with title, company, description, salary, location, remote status, expiry, rating, generated cover letter, and an application status field (To-Do / Applied / etc.).

## Tech Stack

| Component | Tool |
|---|---|
| Orchestration | [n8n](https://n8n.io) |
| Job scraping | [Apify](https://apify.com) – Indeed Jobs Scraper |
| Job scoring | OpenAI GPT-5-mini |
| Cover letter drafting | OpenAI GPT-4o-mini |
| Storage / tracking | Google Sheets |

## Setup

1. Import `Indeed_Job_Seeking_Automation.json` into your n8n instance.
2. Connect credentials for:
   - Apify API
   - OpenAI API
   - Google Sheets OAuth2
3. Update the **Run an Actor** node's `customBody` with your target query, location, and country.
4. Replace the candidate resume text inside the two OpenAI nodes' prompts with your own profile.
5. Point the **Google Sheets** node at your own spreadsheet (create one with matching column headers: Title, Job Description, Platform, Link, Date, Rating, Company Name, Cover Letter, Salary, City, Expired, Remote, Status).
6. Remove the **Limit** node once you've tested the flow end-to-end and are ready to run it live.
7. Activate the workflow and set your preferred schedule interval.

## Notes

- The scoring rubric and cover letter prompt are fully customizable — adjust point weights or resume content to fit any role search, not just AI/automation roles.
- The `Link` field is used as the matching column in Google Sheets, so re-running the workflow won't create duplicate rows for jobs already logged.
- This is a self-built demo project, not a client deliverable — a working example of an AI-powered job discovery and screening agent.

## Author

**Sohag Gain**
Founder & CEO, AI Smart Galaxy — AI Automation, AI Agents & Business Workflow Consulting
🌐 [aismartgalaxy.com](https://www.aismartgalaxy.com) | [LinkedIn](https://bd.linkedin.com/in/sohaggain)
