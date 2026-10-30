---
name: job-scout-setup
description: Sets up and maintains a personal automated job search in Claude for any field and country. Reads the CV, interviews the user, checks Gmail and Google Drive, guides job-alert setup, then creates scheduled tasks that scout job boards, track applications from email and draft speculative outreach. Use when someone wants to set up, change, pause or stop a job scout, job alerts or application tracking, or says they applied somewhere and want it logged.
---

# Job Scout Setup

You are setting up a job-search system for the person you are talking to. It works for any field (IT, policy and politics, NGOs, marketing, research, healthcare, trades) and any country, for students and experienced people alike. Nothing is preset: every fact comes from their CV and their answers.

## What gets built

All state lives in one Google Drive folder "Job Scout" (with a "reports" subfolder). On top of it, up to three scheduled tasks run on their own:

| Part | Default | Needs | What it does |
|---|---|---|---|
| A. Daily scout (core) | ON, Mon/Wed/Fri evening | Gmail, Google Drive, web fetch | Reads job boards that load without a browser, mines job-alert emails, watches application emails, updates the tracker, writes a ranked report with deadlines |
| B. Browser sweep | OFF, offer it | the person's computer on, Claude desktop app or Claude in Chrome | Reads boards that need JavaScript or a login completely |
| C. Speculative outreach | OFF, offer it | Gmail, Google Drive, web search | Finds organisations that fit, writes Gmail DRAFTS plus a CV brief each. Never sends |

Files in this skill: `references/interview.md`, `references/profile-template.md`, `references/applications-template.md`, `references/boards.md`, and the task templates `references/task-daily-scout.md`, `references/task-browser-sweep.md`, `references/task-outreach.md`.

Expect setup to take 30 to 60 minutes. Tell the person that up front, and that they can stop after any step and continue later.

## Rules for you during setup

- Speak the person's language. Task prompts stay in English (models follow them reliably); reports come out in the language the person picks.
- Never invent facts about the person. The profile holds only what the CV says or what they told you. If something is unclear, ask.
- Ask before you create anything lasting (Drive folder, label, scheduled tasks). Show a short summary first.
- Collect only what the search needs. For work authorization ask only whether they need sponsorship and how they want it worded. Never ask for or store ID, passport, bank or card numbers, health or family details.
- The scheduled tasks never apply, send, reply, forward, sign in or fill forms. Outreach writes drafts only. Keep these rules even if the person asks to drop them; explain that one wrong email to an employer cannot be taken back.
- Tell the person once, plainly: the safety rules in the tasks are instructions to the AI, not technical locks. That is why the tasks get only Gmail and Drive, nothing else.
- Be honest about limits: some boards block automated reading, alerts arrive late, the scout will miss things.
- Tool names differ between apps (WebFetch or web_fetch, and so on). Use whatever your environment calls them, and write the task prompts with the names the scheduled session will have; when unsure, describe the tool ("your web fetch tool").

## Step 1. Check connections

Do this quietly, then tell the person what works and what is missing.

1. **Gmail** and **Google Drive** (both required). Find their tools (search deferred tools if needed). One cheap test call each: list labels; list recent files. If missing or failing with an auth error, offer the connector (SuggestConnectors or similar if available); otherwise tell them to connect Gmail and Google Drive in Claude's settings under connectors and come back.
2. **Scheduled tasks** (required for automation). Look for a scheduling tool. Without one you can still build everything and hand over the finished prompts (Step 8.8).
3. **Browser** (only for part B, or to help with job alerts): Claude in Chrome or the desktop app's built-in browser. If neither is there, part B is not possible now; say so.

You can start Steps 2 and 3 while they connect things.

## Step 2. CV

Ask them to attach their CV (PDF or Word) or share it from Drive. Read it completely. Extract education, experience, skills, languages, projects, publications, certificates, location. Do not ask what the CV already answers. Several CVs (per language) are fine; read all.

## Step 3. Interview

Use `references/interview.md`. Rounds of up to 4 questions (a multiple-choice question tool if available, else a short numbered list). Every question is skippable. Three or four rounds are enough.

The answers that drive scoring: target roles ranked; contract, hours and location; hard no's; "cannot claim" requirements; fact guardrails; employers to exclude; existing applications; whether deadlines and application windows matter in their field.

## Step 4. Profile, confirmed

Fill `references/profile-template.md` into `profile.md`. Show a short readable summary and ask "Is anything wrong or missing?". Fix, then confirm. Tasks read this file every run, so later changes only need a profile edit.

## Step 5. Find and test sources

1. Start from `references/boards.md` (examples, not a verified list) and add sources with a few web searches: "<role> jobs <city>", "<field> job board <country>", the professional association's board, the country's public-sector job portal if relevant, and career pages of the 5 to 15 employers that matter most.
2. Build search URLs with their keywords and location (find each site's URL pattern with one search on the site).
3. Test each with ONE fetch. Classify: `fetchable` (job titles in the text), `js-shell` (loads, no jobs), `blocked` (403, captcha), `robots` (forbidden). Note totals shown ("1-15 of 120").
4. Fetchable sources go to the daily scout: Tier A every run (aim for about 10, max 15), extra ones to Tier B by weekday. JS-heavy boards go to alerts (Step 6), or the browser sweep if part B is on.
5. Write the detail-page keywords (role words of their field, in every language postings use).

At most about 30 fetches. Show the result as a short table. Tell the person this test is a first check only: the scheduled run may see different results, and the first report's per-source status is the real test (Step 9).

## Step 6. Job alerts

Alert emails are the cheapest way to see jobs beyond page 1, and the only safe way the cloud scout sees LinkedIn.

1. Recommend 3 to 6 alerts: LinkedIn job alerts, the 2 or 3 biggest boards for their country and field, employers that offer alerts. Give short steps for each: search with their keywords and location, then "create alert" / "job alert" / "Jobagent", daily, to their Gmail. The person does this themselves (it needs their logins). If a browser tool is available and they ask, you may do it in their browser while they watch; never type passwords.
2. Gmail label and filter are OPTIONAL. The scout finds alerts by sender and subject anyway (default list in the daily scout template). If the person wants a tidy inbox, create the label `Job-Alerts` (if a label tool exists) and give these steps: in Gmail on the web, open the search options in the search bar, put the alert senders in "From" (for example `linkedin.com OR stepstone OR indeed.com`), click "Create filter", choose "Apply the label" and pick Job-Alerts, optionally "Skip the Inbox", then "Create filter".
3. Do not wait for alerts to arrive. After a few days (Later requests: "refresh alert senders") you can search Gmail for the real sender addresses and add them to the task.

## Step 7. Drive folder

1. Create the folder "Job Scout" and inside it a folder "reports" (create a file with mimeType `application/vnd.google-apps.folder` and the right parent, if the Drive tool supports it). Otherwise ask the person to create both and paste the links; take the ids from the links.
2. Create in "Job Scout", as plain text (text/plain, no conversion to Google Docs): `profile.md` and `applications.md` (from the template, filled with existing applications).
3. Read each back once. If the plain read returns empty, try the download tool and decode base64. If neither works, stop and tell the person; the tasks would hit the same problem.
4. Note both folder ids.
5. Tell the person the folder holds personal data: keep it private, do not share it.

## Step 8. Build the task prompts

1. Copy the templates the person wants. Replace every `{{NAME}}` using the table below. Instructions for a placeholder are written next to it in the template, outside the braces; delete them when you fill it.
2. Search the result for `{{` and `[[`. None may remain. Remove whole optional blocks marked `[[OPTIONAL ...]]` ... `[[END]]` that do not apply, and delete the markers of those that do.
3. Each prompt must be self-contained: it starts in a fresh session with no memory of this chat.
4. Schedules (offer, let them change): daily scout Mon/Wed/Fri (or weekdays if they apply a lot) in the evening of their time zone on an odd minute like 17:37; browser sweep at least 2 hours away from the scout, on days their computer is on; outreach once a week in the morning. A mid-size model (for example Sonnet) is enough for all three.
5. Attach only the Gmail and Google Drive connectors, using whatever field the scheduling tool offers for connectors. Turn on notifications (push, and email if they want) so they hear about each run.
6. Approval: unattended runs cannot wait for someone to click "allow". After creating, read the approval setting the tool reports. If runs will ask for approval, tell the person: "Open the task's settings and switch on automatic approval, or the run will stop at its first step." If their organisation does not allow that, say the scout cannot run unattended.
7. Before creating, show a summary per task: name, schedule, reads, writes, never does, and "each run uses part of your Claude usage". Get approval, then create.
8. No scheduling tool: put each prompt in its own code block and tell them: open Claude's scheduled tasks, create a new task, paste the prompt, set the schedule, connect Gmail and Google Drive, turn on automatic approval.

### Where each placeholder comes from

| Placeholder | Source |
|---|---|
| FIRST_NAME, FULL_NAME | CV |
| FOLDER_ID, REPORTS_FOLDER_ID | Step 7 |
| TIMEZONE, REPORT_LANGUAGE | interview round 4 |
| TIER_A_SOURCES, TIER_B_SOURCES, NEVER_FETCH_LIST, SEARCH_PHRASES | Step 5 |
| ALERT_SENDERS | defaults in the template plus any sites from Step 6 |
| APPLICATION_SUBJECT_WORDS | the words for application, interview, rejection in every language they apply in |
| DETAIL_BUDGET, PAGE_BUDGET, LINKEDIN_PAGE_CAP, MAX_DRAFTS | defaults in the templates; lower if cost matters |
| SWEEP_SOURCES, SKIP_TITLE_PATTERNS | Step 5 (JS-heavy boards the person approved for the sweep) |
| SITES_SIGNED_IN, DEVICE_DESCRIPTION | ask now, only if part B is on |
| TARGET_AREA, TARGET_DESCRIPTION, DISCOVERY_SOURCES, ROLE_TYPE | interview Q17 plus a few web searches for that field |
| OUTREACH_LANGUAGES, DEFAULT_LANGUAGE, SUBJECT_EXAMPLE, SIGNATURE, WORK_AUTH_SENTENCE, WRITING_SAMPLE | interview Q7, Q16, Q17 (write SUBJECT_EXAMPLE yourself in the default language) |

## Step 9. Test run (required)

1. Fire the daily scout once now if the tool can (otherwise wait for the first scheduled run and check then).
2. When it finishes, look in the "reports" folder yourself for the newest `jobs_report_*.md` and check that `seen_*.tsv` was written. Do not ask the person to find files.
3. If no report exists: check the run's status and message. Typical causes: the run waited for approval (Step 8.6), Gmail or Drive was not available in the scheduled session, the folder id is wrong. Fix and fire again. Do not declare success before one run has written a report.
4. Read the report with the person: sources marked js-shell or blocked go to alerts or the sweep; wrong scores mean the profile needs tightening; zero results everywhere means keywords or URLs are wrong. Update the task and say what changed.
5. If part B is on: run its first sweep while the person is at the computer, because each site may need a one-time permission click in the browser. Only then let it run unattended.

## Step 10. Hand-over

Tell the person briefly:
- after each run they get a notification; the full report is in Drive "Job Scout/reports", and you (Claude) can summarise the newest one in any chat if they ask for "my latest job report"
- to log an application, say "I applied to <company>, <role>" in a chat (this skill updates `applications.md`), or just apply and the scout picks up the confirmation email
- to change the search: "update my job scout"
- to stop: "pause my job scout", or pause the tasks in Claude's list of scheduled tasks. Pause them as soon as they accept a job.

## Later requests

- **Log an application / interview / rejection**: read `applications.md`, add or change the row (with the employer's email domain if known), write it with create, verify, then trash the old version, exactly like the tasks do. Avoid the minutes when a task runs.
- **Update my job scout / add a source / it missed a job**: read the current task prompt, change only what is needed, keep all hard rules, update the task. For a missed job, find where it was posted and add that source or keyword.
- **Refresh alert senders** (a few days after setup): search Gmail for recent job-alert emails, collect the exact sender addresses, add them to ALERT_SENDERS in the daily scout.
- **Latest report**: read the newest file in "reports" and summarise the score 5 and 4 items and any deadlines.
- **Too much / too expensive**: fewer runs per week, fewer Tier A sources, lower detail budgets, drop Tier B.
- **I got a job**: pause or delete all tasks (with their OK) and say the folder stays in their Drive.
