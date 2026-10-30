# Job Scout: a job search that keeps running while you do other things

This is a skill for Claude. After one setup chat (30 to 60 minutes, you can pause it) you get:
- a report of new matching jobs a few times a week, scored 1 to 5 against what you want, with deadlines
- an application tracker that updates itself from your Gmail (confirmations, rejections, interview invites)
- optional: speculative application emails to organisations you choose, saved as Gmail drafts for you to check and send
- optional: a deeper sweep of sites that need your browser

It works for any field (tech, politics and policy, NGOs, research, anything) and any country. Nothing is sent or submitted without you.

## What you need
- A paid Claude plan with scheduled tasks
- Gmail and Google Drive connected to Claude (setup helps you)
- Your CV (PDF or Word)
- For the browser sweep only: your computer on at the scheduled time with the Claude desktop app or the Claude in Chrome extension. Note that LinkedIn forbids automated reading and may restrict accounts; email job alerts are the safe alternative and the default.

## Install
1. Download [job-scout-setup.zip](job-scout-setup.zip) from this repo (click it, then the download button) and upload it under Skills in Claude's settings. If Claude does not find the skill later, check that code execution is turned on in the same settings.
2. Start a new chat and write: "Set up my job scout."
3. Attach your CV when asked, answer the questions, create the job alerts it suggests.

## Good to know
- Each run uses part of your Claude usage. If that is too much, say "run my job scout less often".
- Your files live in a Google Drive folder "Job Scout" (reports in "Job Scout/reports"). It holds personal data, so keep it private. To read the newest report, just ask Claude "show my latest job report".
- Applied somewhere? Tell Claude "I applied to <company>, <role>".
- To change what it looks for: "update my job scout".
- To stop: "pause my job scout", or pause the tasks in Claude's list of scheduled tasks. Do it once you have a job.
